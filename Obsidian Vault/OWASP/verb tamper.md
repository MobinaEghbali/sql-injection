```shell
curl -v "URL" -X POST

curl -v "URL" -X HEAD

curl -v "URL" -I
```
temper
```shell
#file name .bash_profile    for tamper
export BASH_SILENCE_DEPRECATION_WARNING=1

PATH=/usr/local/go/bin:$PATH

# go path configurations
export GOPATH=$HOME/go/ # don't forget to change your path correctly!
export PATH=$PATH:$GOPATH/bin
export PATH=$PATH:$GOROOT/bin

# my personal bin
export PATH=$PATH:~/bin
export PATH=$PATH:/opt/homebrew/bin/

function tmp(){
	rm -rf /tmp/tmp.txt;touch /tmp/tmp.txt;open /tmp/tmp.txt
}
tamper(){
	echo $1
	for method in GET POST PUT DELETE HEAD; do
		echo $method $(curl -s -k $1 -X $method -o /dev/null -w '%{http_code} - %{size_download}')
	done
}
```