# String replacemnt

## Question

Within the Stratos DC, the backup server holds template XML files crucial for the Nautilus application. Before utilization, these files require valid data insertion. As part of routine maintenance, system admins at xFusionCorp Industries employ string and file manipulation commands.

Your task is to substitute all occurrences of the string Text with LUSV within the XML file located at /root/nautilus.xml on the backup server.

## Answer

We can use `sed` command to replace all the words to another
```bash
sed -i "s/Text/LISV/g" nautilus.xml
```

## Additional Details

```bash
-i  Edit inplace (0verwrite file)
-n	Suppress automatic printing of lines.
-e	Allows multiple commands.
-f	Reads sed commands from a file.
-r	Use extended regular expressions.


/g - Replacing all the Occurrence of the Pattern in a Line
```

## Brief Description About the Environment

**sed command in linux?**

The `sed` command in Linux is a stream editor used to perform basic text transformations on an input stream (a file or input from a pipeline). Common uses include searching, replacing, inserting, and deleting text.

**Example usage:**

- Replace "foo" with "bar" in a file:
  ```bash
  sed 's/foo/bar/g' input.txt
  ```

- In-place replacement (modifies the file directly):
  ```bash
  sed -i 's/foo/bar/g' input.txt
  ```

- Delete lines containing "pattern":
  ```bash
  sed '/pattern/d' input.txt
  ```

**Syntax:**
```bash
sed [options] 'command' file
```

