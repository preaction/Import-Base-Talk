
# Get to the Point With Import::Base

<http://preaction.github.io/Import-Base-Talk/>

by Doug Bell (preaction)  
<a href="http://twitter.com/preaction"><i class="fa fa-twitter"></i> @preaction</a>  
<a href="http://github.com/preaction"><i class="fa fa-github"></i> preaction</a>  
[Chicago.PM](http://chicago.pm.org)  

Source: <https://github.com/preaction/Import-Base-Talk>  
License: [CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/legalcode)

------

# How many modules do you use?

---

```
use strict;
use warnings;
use feature qw( :5.24 signatures );
no warnings 'experimental::signatures';
```

---

```
use strict;
use warnings;
use feature qw( :5.24 signatures );
no warnings 'experimental::signatures';
use autodie;
use List::Util qw( reduce pairgrep );
```

---

```
use strict;
use warnings;
use feature qw( :5.24 signatures );
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

