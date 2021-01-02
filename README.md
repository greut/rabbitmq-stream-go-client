# GO stream client
---
A POC client for [RabbitMQ Stream Queues](https://github.com/rabbitmq/rabbitmq-server/tree/master/deps/rabbitmq_stream)

### How to use
---

```golang
		fmt.Println("Connecting ...")
    	var client = stream.Client{} // create Client Struct
    	err := client.Connect("rabbitmq-stream://guest:guest@localhost:5555/%2f") // Connect
    	if err != nil {
    		fmt.Printf("error: %s", err)
    		return
    	}
    	fmt.Println("Connected!")
    	streamName := "my-streaming-queue"
    	err = client.CreateStream(streamName) // Create the streaming queue
    	if err != nil {
    		fmt.Printf("error: %s", err)
    		return
    	}
    
    	err = client.DeclarePublisher(0, streamName) 
    	if err != nil {
    		fmt.Printf("error: %s", err)
    		return
    	}
    	var arr []*amqp.Message
    	for z := 0; z < 100; z++ {
    		arr = append(arr, amqp.NewMessage([]byte("hello world_"+strconv.Itoa(z))))
    	}
    	err = client.BatchPublish(arr)
    	if err != nil {
    		fmt.Printf("error: %s", err)
    		return
    	}

```
 