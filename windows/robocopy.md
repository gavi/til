# Robocopy to keep files in Sync

`robocopy` is a great tool on Windows since vista

### Test before copy

If you would like to test what would happen if you execute the command, Append `/L`. This would make sure no action is actually performed

### Move all files before a certain date

```
robocopy Source Destination /S /MINAGE:500 /MOV /Z /XO 
``` 
- `/S` Copies subdirectories
- `MINAGE:500` only picks up files older than 500 days
- `MOV` ensures, files are moved 
- `/Z` 	ensures you can restart if stopped in middle
- `/XO` to exclude older files


### Mirror a folder remotely

WARNING: This command will delete files in the destination if they are not in the source

```
robocopy Source Destination /S /MIR /XD Exclude

```

- `MIR` For mirror
- `XD` Exclude folders
