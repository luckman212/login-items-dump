<img src=icon.png width=128>

# login-items-dump

Inspired by [this AskDifferent post](https://apple.stackexchange.com/questions/465920/how-to-determine-details-of-backgound-process), this is a little mashup of `sfltool` + `awk` that enumerates, parses and outputs details about your Mac's Startup Items/Login Items, which are stored in a *Background Task Management* aka **BTM** database.

This information is normally shown with very limited info in System Settings → General → Login Items. For example:

<img src=image1.png width=478>

Note how it's missing any information about what or where those programs are, Bundle IDs, file paths, etc. Not very useful.

### Setting up

Clone this repo (if you don't know how to do that, click the green **Code** button above, then **Download ZIP**) and copy the `login-items-dump` script to your `/usr/local/bin` directory. You can then run the command from a Terminal.

### Usage

Open a Terminal and type
```
login-items-dump
```

> **N.B.** *executing `sfltool dumpbtm` requires root permissions, so you will likely be asked for your password. For unattended scripting, you can use `SUDO_ASKPASS` to avoid the password prompt, but that's beyond the scope of this README.*

The output is tab-separated, and contains 5 columns which should contain useful information:

- UUID
- Name (DeveloperName) (daemons are prefixed with a 👿 emoji)
- BundleID
- URL (typically a URL-encoded file path)
- ExecPath

You can parse the output further with your favorite unix tools like `grep`, `sed`, or `awk`.

> **Note about Daemons**
> If you see _"affects all users"_ in small text below the item in the Settings app, it indicates that item is a daemon. That 👿 may help you identify such items when there are multiple items with the same Name. For example:
> 
> <img src=image3.png width=464>
> 
> <img src=image4.png width=838>

### Examples

Output in neatly formatted columns
```
( printf "UUID\tName\tBundleID\tURL\tExecPath\n"; login-items-dump; ) | column -s$'\t' -t
```

To squeeze and truncate the output so it fits better on a single line, try

```
login-items-dump | sed 's/\t/  /g' | cut -c-$((COLUMNS-5))
```

To search for Google stuff
```
login-items-dump | grep -i google
```

If you see something you don't like, you can reset the entire BTM database (careful, you will have to reset all your startup apps) with:
```
sfltool resetbtm
```

For more information about background task management, you may want to read Apple's [Manage login items and background tasks on Mac](https://support.apple.com/guide/deployment/manage-login-items-background-tasks-mac-depdca572563/web) page.

### Screenshot showing a portion of the program's output

<img src=image2.png width=1016>
