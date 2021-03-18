```applescript
on alfred_script(q)

set pre to "echo '"
set suf to "' | quicktype  -l objc --just-types --class-prefix FY --features interface"

set cmd to pre &q &suf

	tell application "terminal"
		activate
		do script cmd
	end tell

end alfred_script

```
