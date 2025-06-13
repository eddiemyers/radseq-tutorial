# Command Line Basics for Bioinformatics

This quick-start guide covers essential command-line tools used throughout this project. It assumes you're working on a Unix-based system (Linux or macOS) or using a terminal in a conda environment.

---

##  File System Navigation

```bash
pwd            # Show current directory (print working directory)
ls             # List files and folders
ls -l          # Long listing format
cd folder/     # Change directory into "folder"
cd ..          # Go up one directory
cd ~/          # Go to your home directory
mkdir newdir   # Make a new directory
rm file.txt    # Remove a file
rm -r folder/  # Remove a folder and its contents
cp file1 file2 # Copy a file
mv old/ new/     # Rename or move a file
```

## Working with Files

```bash
cat file.txt           # View entire file contents
less file.txt          # Scroll through long files (q to quit)
head file.txt          # Show first 10 lines
tail file.txt          # Show last 10 lines
tail -f log.txt        # Continuously follow updates to a file (great for logs)
wc -l file.txt         # Count lines in a file
```

## Searching and Filtering

```bash
grep "pattern" file.txt     # Find lines matching a pattern
grep -i "pattern" file.txt  # Case-insensitive search
sort file.txt               # Sort lines alphabetically
uniq file.txt               # Remove duplicates (usually after sort)
cut -f1,3 file.tsv          # Extract columns 1 and 3 from tab-delimited file
awk '{print $1, $3}' file   # Print specific fields (flexible alternative to cut)
```

## Working with Archives and Permissions

```bash
tar -xzvf archive.tar.gz  # Unzip and extract .tar.gz file
chmod +x script.sh        # Make a script executable
./script.sh               # Run a script in the current directory
```

##

| Mode         | Command     | What it does                  |
| ------------ | ----------- | ----------------------------- |
| Command mode | `i`         | Enter **insert mode** to type |
| Insert mode  | Press `Esc` | Return to command mode        |
| Command mode | `:w`        | Save the file                 |
| Command mode | `:q`        | Quit                          |
| Command mode | `:wq`       | Save and quit                 |
| Command mode | `:q!`       | Quit without saving           |
