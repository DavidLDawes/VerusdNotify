package main

import (
  "net/http"
)

// For block notifications from verusd, pass in a curl command to call here
// URL should have the %s blockhash passed in via curl
// localhost:8080/block?blockhash=%s
func block(w http.ResponseWriter, r *http.Request) {
  keys, ok := r.URL.Query()["blockhash"]
  if !ok || len(keys[0]) < 1 {
    // log.Println("Url Param 'blockhash' is missing")
    return
  }
  blockhash := keys[0]
  // Code proper response: bookkeeping? Do something?
  // Use the verusd go lib as needed anyway
  // intrepret the blockhash, do all the things
  w.Write([]byte(blockhash))
}

// For wallet notifications form verusd, pass in a curl command to call here
// with /wallet?txid=%s passed in on the URL.

func wallet(w http.ResponseWriter, r *http.Request) {
  keys, ok := r.URL.Query()["txid"]
  if !ok || len(keys[0]) < 1 {
    // log.Println("Url Param 'txid' is missing")
    return
  }
  txid := keys[0]  
  // intrepret the txid, do all the things
  w.Write([]byte(txid))
}

func main() {
  http.HandleFunc("/wallet", wallet)
  http.HandleFunc("/block", block)
  if err := http.ListenAndServe(":8080", nil); err != nil {
    panic(err)
  }
}

// To use this, run verusd like so:
// verusd {usual verusd stuff} -blocknotify `curl localhost:8080/block?blockhash=%s` -walletnotify=`curl localhost:8080/wallet?txid=%s`
//
//  -blocknotify -verusd=`curl localhost:8080/block?%s`
//  -blocknotify=<cmd>
//       Execute command when the best block changes (%s in cmd is replaced by block hash)
//
//  -walletnotify=`curl localhost:8080/wallet?%s`
//  -walletnotify=<cmd>
//       Execute command when a wallet transaction changes (%s in cmd is replaced by TxID)

