<img src=icon.png width=128>

# login-items-dump

Little mashup of `zsh` + `sfltool` + `awk` that enumerates and outputs the obfuscated list from System Settings → General → Login Items, otherwise known as Background Task Management aka **BTM**.

For example, here's the info shown in the Settings app:

<img src=image1.png width=478>

Note how it's missing any information about what or where those programs are, Bundle IDs, etc. Not very useful.

If you clone this repo (if you don't know how to do that, click the green **Code** button above, and then **Download ZIP**) then copy the `login-items-dump` script to your `/usr/local/bin` directory, you can run this command from a Terminal:

```
login-items-dump
```

> It requires root permissions, so you may be asked for your password.

<img src=image2.png width=1163>

The output is tab-separated, and contains 5 columns which should contain useful information:

- UUID
- Name (DeveloperName)
- BundleID
- URL (typically a URL-encoded file path)
- ExecPath

You can parse it further with your favorite unix tools like `grep`, `sed`, `awk`.

### Examples

To squeeze and truncate the output so it fits better on a single line, try

```
login-items-dump | sed 's/\t/  /g' | cut -c-$COLUMNS
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
