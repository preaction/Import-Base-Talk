
# Get to the Point With Import::Base

<http://preaction.github.io/Import-Base-Talk/>

<div style="width: 50%; float: left">

[Source on <i class="fa fa-github"></i>](https://github.com/preaction/Import-Base-Talk)<br/>

[CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/legalcode)<br/>

</div>

<div class="width: 50%; float: left">

by [Doug Bell (preaction)](http://preaction.me)<br>
<a href="http://twitter.com/preaction"><i class="fa fa-twitter"></i> @preaction</a><br/>
<a href="http://github.com/preaction"><i class="fa fa-github"></i> preaction</a><br>
[Chicago.PM](http://chicago.pm.org)<br>

</div>

------

# How many modules do you use?

---

```
use strict;
use warnings;
```

---

```
use v5.24;
use warnings;
use feature qw( signatures );
no warnings 'experimental::signatures';
```

---

```
use v5.24;
use warnings;
use feature qw( signatures );
no warnings 'experimental::signatures';
use autodie;
use List::Util qw( reduce pairgrep );
```

---

```
use v5.24;
use warnings;
use feature qw( signatures );
no warnings 'experimental::signatures';
use autodie;
use List::Util qw( reduce pairgrep );
use Try::Tiny;
use Log::Any;
```

------

# Classes

---

```
use Moose;
use Types::Standard qw( :all );
use My::Types qw( :all );
```

---

```
use Moose;
use Types::Standard qw( :all );
use My::Types qw( :all );
use feature qw( :5.24 signatures );
no warnings 'experimental::signatures';
use autodie;
use List::Util qw( reduce pairgrep );
use Try::Tiny;
use Log::Any;
```

------

# Roles

---

```
use Moose::Role;
use Types::Standard qw( :all );
use My::Types qw( :all );
use feature qw( :5.24 signatures );
no warnings 'experimental::signatures';
use autodie;
use List::Util qw( reduce pairgrep );
use Try::Tiny;
use Log::Any;
```

------

# It's a Mess!

---

# Now switch to Moo...

---

`ack -g --perl pm lib | xargs sed -i.bak s/Moose/Moo/g # and pray`

---

`find lib -name '*.bak' -delete`

------

![](images/eggy-mess.gif)

# There Has To Be A Better Way

------

# There Is!

------

# Import::Base

---

# All your modules in one place!

------

# Create default imports

---

```
package My::Base;
use parent 'Import::Base';
our @IMPORT_MODULES = (
    'strict', 'warnings',
    feature => [ ':5.24', 'signatures' ],
    '-warnings' => [ 'experimental::signatures' ],
    'List::Util' => [qw( reduce pairgrep )],
);
```

---

```
use My::Base;
```

Imports strict, warnings, feature :5.24, sub signatures, and List::Util

------

# Create import bundles

---

```
package My::Base;
use parent 'Import::Base';
our %IMPORT_BUNDLES = (
    Class => [
        'Moo',
        'Types::Standard' => [ ':all' ],
        'My::Types' => [ ':all' ],
    ],
    Role => [
        'Moo::Role',
        'Types::Standard' => [ ':all' ],
        'My::Types' => [ ':all' ],
    ],
);
```

---

```
package My::Class;
use My::Base 'Class';
```

Imports strict, warnings, feature :5.24, sub signatures, and List::Util.

Also imports Moo, Types::Standard, and My::Types

---

```
package My::Role;
use My::Base 'Role';
```

Imports strict, warnings, feature :5.24, sub signatures, and List::Util.

Also imports Moo::Role, Types::Standard, and My::Types

------

# Good For Testing!

---

```
our %IMPORT_BUNDLES = (
    Test => [
        'Test::More', 'Test::Deep',
        'Test::Fatal', 'Test::Differences',
        'Test::Warnings', 'My::Test',
    ],
);
```

---

```
use My::Base 'Test';
```

Imports strict, warnings, feature :5.24, sub signatures, List::Util and
all those test modules

------

# Turn This

---

```
use strict;
use warnings;
use feature qw( :5.24 signatures );
no warnings 'experimental::signatures';
use List::Util qw( reduce pairgrep );
use Moo;
use Types::Standard qw( :all );
use My::Types qw( :all );
use Try::Tiny;
use Log::Any;
```

------

# Into This

---

```
use My::Base 'Class';
```

------

# With Import::Base

