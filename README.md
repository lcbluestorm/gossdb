# Gossdb
a ssdb cache for beego written in go

# Examples

    package main

    import (
        "fmt"
        "github.com/astaxie/beego/cache"
        _ "github.com/lcbluestorm/ssdb-cache/ssdb"

        "time"
    )

    func main() {
      ssdb, err := cache.NewCache("ssdb", `{"conn": "127.0.0.1:8888"}`)
        if err != nil {
            fmt.Printf("init err\n")
        }
        // put
        timeoutDuration := -10 * time.Second
        if err = ssdb.Put("ssdb", "1", timeoutDuration); err != nil {
            fmt.Printf("put err\n")
        }

       // exist
        exist := ssdb.IsExist("ssdb")
        if exist {
            fmt.Printf("existed\n")
        }

        // get
        if v := ssdb.Get("ssdb"); v != nil {
            fmt.Printf("value:%s\n", v)
        }
       //inc/dec test done
       if err = ssdb.Incr("ssdb"); err != nil {
           fmt.Printf("incr err")
        }

        if err = ssdb.Decr("ssdb"); err != nil {
            fmt.Printf("decr error")
         }

        // test del
         if err := ssdb.Delete("ssdb"); err == nil {
             if !ssdb.IsExist("ssdb") {
                fmt.Printf("delete successful\n")
            }
        }

        //test GetMulti
        if err = ssdb.Put("ssdb1", "3", timeoutDuration); err != nil {
            fmt.Printf("put err")
        }
        vv := ssdb.GetMulti([]string{"ssdb", "ssdb1"})

        fmt.Printf("getmulti : %s\n", vv)

        // test clear all done
        if err = ssdb.ClearAll(); err != nil {
            fmt.Printf("clear all err")
        }
    }
