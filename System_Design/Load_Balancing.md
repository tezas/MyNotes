# LOAD BALANCING

## Consistent hashing

Request ID: 0 to m-1;<br/>
Hash(X) = m1;<br/><br/>

To decide target server from N servers: A = m1%N;<br/>

Request Id X will go to server A.<br/>

