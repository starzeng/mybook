# Connection sampler

## TCP connection scenario 1: successful TCP connection in UI mode

- Open JMeter, and create a threadgroup, and rename it to 'MQTT connections', then add a 'MQTT Connection Sampler' under 'MQTT connections'.
- In "Server name or IP", input the MQTT server address that installed in pre-steps.
- In "Port number", input the port number that used by MQTT server.
- In "Keep alive(s)", input 20, which means that the ping packet will be sent every 20 sec.
- In "Connection keep time(s)", input 60, which means that the connection will keep 1 minute after connected successfully.
- Keep rests of controls unchanged.
- Right click thread group 'MQTT connections', then add 'View Results Tree', save the test script.
- Click 'Run' button in toolbar, make sure no exceptions are found in JMeter log.
- Make sure the sampler results in 'View Results Tree' are correct.
- Open EMQ server admin console and make sure:
  1) The connection was created successfully. 
  2) After 20 seconds, a ping packet should be found. 
  3) The connection keeps 1 minutes, and a disconnect packet should be found.
- Click thread group 'MQTT connections', and change 'Number of Threads (users)' to 50, and validate conn/disconn/ping packets numbers in EMQ admin console.

## TCP connection scenario 2: successful TCP connection in none-UI mode

- Restart EMQ server.
- Close JMeter, and run the previous testcase in none-ui mode, validate conn/disconn/ping packets numbers in EMQ admin console.

## TCP connection scenario 3: successful TCP connection in UI mode and break the test during running

- Open JMeter and test script created in last step.
- In "Connection keep time(s)", change 60 to 120, so the connection will keep 2 minutes after connecting successfully.
- Run the test, and keep it running in 1 min, validate conn/ping packets numbers in EMQ admin console.
- Stop the test, and make sure the test can be finished soon.

## TCP connection scenario 4: successful TCP connection in non-UI mode and break the test during running

- Open JMeter and test script created in last step.
- In "Connection keep time(s)", change 60 to 120, so the connection will keep 2 minutes after connecting successfully.
- Close JMeter, and run the test in non-ui mode, and keep it running in 1 min, validate conn/ping packets numbers in EMQ admin console.
- Stop the test by ctrl+c, and make sure the test can be finished soon.

## TCP connection scenario 5: failed TCP connection in UI mode

- Open JMeter and test script created in last step.
- In "Server name or IP", change to an IP address that can be accessed by your JMeter but not install with EMQ server, save the script, and run it.
- Validate the script is failed quickly, and sampler results in 'View Results Tree' displayed with failed message.

## TCP connection scenario 6: failed TCP connection in none-UI mode

- Open JMeter and test script created in last step.
- In "Server name or IP", change to an IP address that can be accessed by your JMeter but not install with EMQ server, save the script.
- Run the script in none-ui mode, make sure the script is failed quickly.

## TCP connection scenario 7: successful TCP connection with user authentication in ui mode

- Change the EMQ configuration to add user authentication, and restart EMQ server.
- Open the last test scenario, and change 'User name' and 'Password' value to configurations in previous step.
- Run the test and validate as last scenario.
- Disable EMQ configuration not to use the user authentication.

## TCP connection scenario 8: successful SSL connection in ui mode

- Change EMQ configuration to support SSL, and restart EMQ server.
- Open the last test script, and remove 'User name' and 'Password' value of 'User authentication' section.
- In 'Port number', change it to the SSL port.
- Select 'SSL' in 'Protocols' dropdown box.
- Save the script, and run it, validate conn/disconn/ping packet num in EMQ admin console.

## TCP connection scenario 9: successful dual-SSL connection in ui mode

- Change EMQ configuration to support dual-SSL, and restart EMQ server.
- Open the last test script, and check 'Dual SSL authentication', follow steps in [Certification files for SSL/TLS connections](https://github.com/emqtt/mqtt-jmeter) section to enable dual SSL configurations.
- Save the script, and run it, validate conn/disconn/ping packet num in EMQ admin console.

## TCP connection scenario 10: 100000 TCP connection

- Change EMQ configuration back to normal TCP connection, and restart EMQ server.
- Open JMeter, and create a threadgroup, and rename it to 'MQTT connections', then add a 'MQTT Connection Sampler' under 'MQTT connections'.
- In "Server name or IP", input the MQTT server address that installed in pre-steps.
- In "Port number", input the port number that used by MQTT server.
- In "Connection keep time(s)", input 600, which means that the connection will keep 10 minutes after connected successfully.
- Keep rests of controls unchanged.
- Save the script and upload it to xmeter.net.
- Run the script with 100000 virtual user and 12 minutes, validate conn/disconn/ping packet num in EMQ admin console.

# Pub sampler

**Please refer to pre-steps for installing EMQ and JMeter MQTT plugin **

## Pub scenario 1: TCP connection * QoS0 * Random string with fixed length

- Create a JMeter test script > add a thread group named 'MQTT Pub Samplers' > add a 'MQTT Pub Samplers'
- Set the correct server address and port.
- Select 'Random string with fixed length' in 'Message type', the 1024 in 'Length' means send the server with random 1024 length of string.
- Add a constant timer and set to 1000.
- Under thread group 'MQTT Pub Samplers', add a 'View Results Tree', and save test script.
- Open an MQTT client, and subscribe the 'test_topic' topic.
- Change the thread group loop count to 20.
- Run test script and then make sure the message is delivered to subscriber, and the 'View Results Tree' displays correct result. Also, validate conn/disconn/ping/pub packet num in EMQ admin console.
- Change the thread group user number to 10. Run the test and validate 'View Results Tree' and conn/disconn/ping packet etc.

## Pub scenario 2: TCP connection * QoS1 * String * Add timestamp in payload

- Restart EMQ server.
- Open the script in last test scenario, and click 'Qos Level' dropdown box, select '1'.
- Select 'String' in 'Message type', and check 'Add timestamp in payload'.
- Input any string in text-area.
- Open an MQTT client, and subscribe the 'test_topic' topic.
- Run test script and then make sure the message is delivered to subscriber, and the 'View Results Tree' displays correct result. Also, validate conn/disconn/ping/pub packet num in EMQ admin console.

## Pub scenario 3: TCP connection * QoS2 * Hex string * Add timestamp in payload

- Restart EMQ server.
- Open the script in last test scenario, and click 'Qos Level' dropdown box, select '2'.
- Select 'Hex string' in 'Message type', and check 'Add timestamp in payload'.
- Input any HEX string in text-area.
- Open an MQTT client, and subscribe the 'test_topic' topic.
- Run test script and then make sure the message is delivered to subscriber, and the 'View Results Tree' displays correct result. Also, validate conn/disconn/ping/pub packet num in EMQ admin console.

## Pub scenario 4: TCP connection * QoS0 * Hex string * User authentication in non-ui

- Configure EMQ with user authentication, and then restart EMQ server.
- Open the script in last test scenario, and click 'Qos Level' dropdown box, select '0'.
- Select 'Hex string' in 'Message type', and uncheck 'Add timestamp in payload'.
- Input any HEX string in text-area.
- Open an MQTT client, and subscribe the 'test_topic' topic.
- Close JMeter, and run test script in non-ui mode and then make sure the message is delivered to subscriber. Also, validate conn/disconn/ping/pub packet num in EMQ admin console.

## Pub scenario 5: SSL * QoS1 * String

- Configure EMQ with SSL authentication, and then restart EMQ server.
- Open the script in last test scenario, and click 'Qos Level' dropdown box, select '1'.
- Click 'Protocols' dropdown box, select 'SSL'.
- Select 'String' in 'Message type', and uncheck 'Add timestamp in payload'.
- Input an string in text-area.
- Open an MQTT client, and subscribe the 'test_topic' topic.
- Close JMeter, and run test script in non-ui mode and then make sure the message is delivered to subscriber. Also, validate conn/disconn/ping/pub packet num in EMQ admin console.

## Pub scenario 6: Dual SSL * QoS1 * Random string with fixed length

- Configure EMQ with dual SSL authentication, and then restart EMQ server.
- Open the script in last test scenario, and click 'Qos Level' dropdown box, select '1'.
- Click 'Protocols' dropdown box, select 'SSL', and check 'Dual SSL authentication'. Follow steps in [Certification files for SSL/TLS connections](https://github.com/emqtt/mqtt-jmeter) section to enable dual SSL configurations.
- Select 'Random string with fixed length' in 'Message type', keep the default value 1024 in 'Length' input box.
- Open an MQTT client, and subscribe the 'test_topic' topic.
- Save and run script, then make sure the message is delivered to subscriber. Also, validate conn/disconn/ping/pub packet num in EMQ admin console.

## Pub scenario 7: TCP connection * QoS0 * Random string with fixed length * 4000 users

- Remove EMQ dual SSL authentication, and then restart EMQ server.
- Open the script in last test scenario, and click 'Qos Level' dropdown box, select '0'.
- Click 'Protocols' dropdown box, select 'TCP'.
- Click 'Qos Level' dropdown box, select '0'.
- Select 'Random string with fixed length' in 'Message type', keep the default value 1024 in 'Length' input box.
- Save and upload the script to xmeter.net. Run the script with 4000 vu and 3 minutes.
- Open an MQTT client, and subscribe the 'test_topic' topic.
- Save and run script, then make sure the message is delivered to subscriber. Also, validate conn/disconn/ping/pub packet num in EMQ admin console.

# Sub sampler

## Sub scenario 1: TCP connection * QoS0 * Payload includes timestamp

- Configure EMQ with TCP connection, and then restart EMQ server.

### Create and run a sub sampler

- Create a JMeter test script > add a thread group named 'MQTT Sub Samplers' > add a 'MQTT Sub Sampler'
- Under thread group 'MQTT Pub Samplers', add a 'View Results Tree', and save test script.
- Set the correct server address and port.
- Check 'Payload includes timestamp'.
- Check 'Debug response'.
- Change 'Loop count' for thread group 'MQTT Sub Samplers' to -1.
- Add a constant timer, and set the timer as 1000 (1 second).
- Save and run the test script.
- After starting below pub script, validate the average message latency time is calculated correctly. Also, validate conn/disconn/ping/pub/sub packet number in EMQ admin console.

### Create and run a pub sampler

- Create a JMeter test script > add a thread group named 'MQTT Pub Samplers' > add a 'MQTT Pub Samplers'
- Under thread group 'MQTT Pub Samplers', add a 'View Results Tree', and save test script.
- Set the correct server address and port.
- Select 'Random string with fixed length' in 'Message type', the 1024 in 'Length' means send the server with random 1024 length of string.
- Check 'Add timestamp in payload'
- Under thread group 'MQTT Pub Samplers', add a 'View Results Tree', and save test script.
- Add a constant timer and set to 1000.
- Change the thread group loop count to 10.
- Run test script and then make sure the message is delivered to subscriber, and the 'View Results Tree' displays correct result. Also, validate conn/disconn/ping packet num in EMQ admin console.
- Change the thread group user number to 10. Run the test and validate 'View Results Tree' and conn/disconn/ping/pub packet etc.

## Sub scenario 2: SSL connection * QoS1 * Payload not include timestamp

- Configure EMQ with SSL connection, and then restart EMQ server.

### Prepare sub sampler

- Open sub sampler in last step, select 'SSL' protocol.
- Uncheck 'Payload includes timestamp' and 'Debug response'.
- Change QoS to 1.
- Save and run the test script.
- After starting below pub script, validate the average message latency time will be always 0. Also, validate conn/disconn/ping/pub/sub packet number in EMQ admin console.

### Prepare pub sampler

- Uncheck 'Add timestamp in payload'.
- Change QoS to 1.
- Run test script and then make sure the message is delivered to subscriber, and the 'View Results Tree' displays correct result. Also, validate conn/disconn/ping packet num in EMQ admin console.

## Sub scenario 3: SSL connection * QoS1 * Payload not include timestamp

- Upload the sub sampler in last scenario to xmeter.net, and run it with 2000 vu.
- Run pub test script and validate conn/disconn/ping packet num in EMQ admin console.