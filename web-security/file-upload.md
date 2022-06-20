# File Upload

## File Upload

### curl

We can use curl to upload files to a website using its form:

```bash
curl -X POST -sD - -F "tip=<shell.zip" -F "name=a" -F "token=f7ab6fe8906a56cc11bc34238be8d1f5efd324c2dceb7f216c512fdea8b17a5e" -F "submit=Send Tip!" -x 127.0.0.1:8080 http://10.10.10.80/?op=upload -H "Referer: http://10.10.10.80/?op=upload" -H "Cookie: admin=1; PHPSESSID=0v5980ekt4tqigv8e2dtlkvp54"
```

Two things to note here:

1. It’s important to use the `-F` \(--form\) option here for the data.
2. Most references to using curl to upload files show using the `@` symbol in front of the filename. This doesn’t work if the site is expecting raw text:

   > To force the ‘content’ part to be a file, prefix the file name with an @ sign. To just get the content part from a file, prefix the file name with the symbol &lt;. The difference between @ and &lt; is then that @ makes a file get attached in the post as a file upload, while the &lt; makes a text field and just get the contents for that text field from a file.

## Bypass File Upload Filters

### Extensions

### Content-Type

### Magic Bytes



