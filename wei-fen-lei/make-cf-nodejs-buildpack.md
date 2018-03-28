# Make CF node.js buildpack

* [https://github.com/cloudfoundry/nodejs-buildpack](https://github.com/cloudfoundry/nodejs-buildpack)

### env
* OS: Ubuntu 16.04LTS
* GO: go version go1.10 linux/amd64
    * https://github.com/golang/go/wiki/Ubuntu

### Steps

1. `git clone https://github.com/cloudfoundry/nodejs-buildpack.git && cd nodejs-buildpack/`
2. `git checkout v1.6.20`
3. `source .envrc`
4. `(cd src/nodejs/vendor/github.com/cloudfoundry/libbuildpack/packager/buildpack-packager && go install)`
5. `buildpack-packager build --cached=true`

### Troubleshoot
1. dependency sha256 mismatch
```
2018/03/27 16:26:27 error while creating zipfile: 
dependency sha256 mismatch: 
expected sha256 3a0877a41d2cd83a669aaa58ffe9352e52639defe4bd038a662ee8fea1778ffa, 
actual sha256 e0ab3ccaad55413324bbef16ebda9ad91cec1f5ee4e49245cb6c644e726cd755
```
發現原因是CF在本機的快取資料夾（~/.buildpack-packager/cache）髒掉，導致在下載dependency時就可能出現錯誤，所以只要刪除暫存資料夾，從新做一次就可以了



