## PUPA Golang SDK

>PUPA provides the following APIs, you can use them in your program. Since all operations read local shared memory and read action is lock-free operation, so don't worry about performance.

>NOTICE: All write actions need to be locked in your program, which means you need to call them serially. Unless you can guarantee that only one write action is called at the same time. (write actions: Delete & Set)



* Initialize the pupa store.

    * filename - A normal file which stores memory data.
    * keyCount - The maximum number of keys that can be stored. It will be ignored if `opType` is specified as `OPRO`.
    Note: keyCount will be ignored if the filename already exists.
    * opType - Operation type, one of `OPRO` and `OPRW`. `OPRO` for read only, `OPRW` for read write.

    ```golang
    func Init(filename string, keyCount int, opType int) error
    ```

* Delete key-value data according to the specified key.
    ```golang
    func Delete(key string) error
    ```


* Get the value of the key. If the key does not exist then error is returned.
    ```golang
    func Get(key string) (string, error)
    ```

* Set key with the value. If the key already exists, the value will be overwritten.
    ```golang
    func Set(key string, value string) error
    ```

* Get the information & statistics about pupa store. It will return data in json format.
    ```golang
    func Stats() (string, error)
    ```

* End to use
    ```golang
    func Fini() error
    ```

**Note:** Please install the library of pupa before using this APIs.


#### Installation

```shell
sh install.sh
```


#### Example

```golang
package main

import (
    "fmt"
    "github.com/agile6v/pupa"
)


func main() {
    var err error
    var key, value, stats string

    err = pupa.Init("./pupa.store", 2, pupa.OPRW)
    if err != nil {
        fmt.Println("error: ", err)
        return

    }

    key = "Hello"
    value = "pupa.PUPA"

    err = pupa.Set(key, value)
    if err != nil {
        fmt.Println("error: ", err)
        return

    }

    stats, err = pupa.Stats()
    if err != nil {
        fmt.Println("error: ", err)
        return

    }

    fmt.Println("** stat: ", stats)

    value, err = pupa.Get(key)
    if err != nil {
        fmt.Println("error: ", err)
        return

    }

    fmt.Println("** Got " + key + " : " + value)

    err = pupa.Delete(key)
    if err != nil {
        fmt.Println("error: ", err)
        return

    }

    stats, err = pupa.Stats()
    if err != nil {
        fmt.Println("error: ", err)
        return

    }

    fmt.Println("** stat: ", stats)

    err = pupa.Fini()
    if err != nil {
        fmt.Println("error: ", err)
        return

    }
}

```