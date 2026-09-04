port fuzz
https://gist.github.com/cihanmehmet/2e383215ea83e08d01478446feac36d8
```shell
ffuf -w wordlist -u https://interactivemap.redacted.com/pdf.axd?url=https://127.0.0.1:FUZZ
```