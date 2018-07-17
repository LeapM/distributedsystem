# PBFT Paper

PBFT is only 3% slower than a standard unreplicated NFS.

Those replies are correct according to linearizability. Before PBFT, the algrithm is either too slow and assume synchrony. It improves the performance
of Rampart and SecureRing by more than an order of magnitude. 

Use one message round to read, and two to execute read-write operation.

public-key is only used on there are faults. it use efficient authentication scheme during the normal operation

1. System Model inlcuding failure assumptions
2. problem solved and state correctness conditions
3. algrithm description
4. result of the experiment
5. related discussion

## System Model

The network may fail to deliver,delay, duplicate and out of order. cryptogrpahic to prevent spoofing and replays and to detect corrupted messages.
contain public-key, message authentication ocde, collision-resistant hash functions. Common practice of signing a digest of a message and appending it to the plaintext of the message rather than the full message
All repics know the others' public key to verify signatures

assume the adversary cannot delay correct node indefinitely. fault node is unable to subvert the cryptographic techniques. 

## Service Properties

Clients and replicas are not faulty if they follow the algorithm and no attacher can forge the signature. 

Saftty is provided regardles sof how many faulty clients are using the service. 

Liveness rely on sychrony. Safety does not rely on synchrony. Delay(t) is the time when a message is sent for the first time and the moment when it is received by its destination

The algorithm doesn not address the problem of fault tolerant privacy. It is possible to use secret sharing scheems to obtain privacy

## Algorithm

set of Replicas by R. WIth more than 3f + 1 replicas, it degrade performance (more and bigger message are being exchanged)

move through a succession of configruation called view. Views are numbrere consecutively. p = v mod |R|, v is the view number. View changes are carried out when it appears that the pirmary has failed
viewsteamped replicaiton and Paxos used a similar approach. 

### roughly how it works

1. client send request to primary
2. primary multicasts the request to the backups.
3. replicas execute the request and send a reply to the client
4. clients watis for f + 1 replies from different replicas when the same result.

they must be deterministic and must start in the same state.  all non faulty replicas agree on a total order for the execution of requests despoite failures

### client
<REQUEST, o, t, c>signed t(timestamp). The reply <REPLY, v, t, c, i, r> signi. V is the current view number, t is the timestamp of requst, i the replica number, c is the client, r the answer

the client waits for f + 1 replies and with the same t and r before acception the result. this ensure that the result is valid. 

if the client doesn't receive replies soon enough, it broadcast the request to all replices. if the primary does not multicast the request, it will eventualy be suspected to be faulty and cause a view change

In this paper we assume that the client waits for **one request to complete before sending the next one**. but we allow a client to make asynchronous request yet preserve ordering contatrains on them.


### normal client operation

state of reach replica includes the state of the service, a message log containing messages the replicas has accepted and an integer denoting the replica's currnet view

if system is overloaded, the message is buffered. The three phase is 
. pre-prepare
. prepar
. commit
the pre-prepare and prepare phase are used to totally order requests sent in the same view even when the primary is fault. <<PRE-PREPARE, v, n, d>sing, M>
d is the m's digest

Request are not include in pre-pare messages to keep them small. pre-prepare message are used as a proof that request was assigned sequence number n in view v in view changes.

backup accept the pre-prepare provided:

1. message is valid(request signature, pre-prepared message are correct and d is the digest for m
2. it is in view v
3. it doesn't have v and n containa differenrt digest
4. the seuence number is between h and H

after accept, backup multicast <prepare, v, n, d, i>sign message to all other replicas and adds both messages to it log. 

a replica(including the primary) accepts prepare message and adds them to its log. 

prepared(m,v,n,i) to be ture iif replic i has inserted in tis glog. The replics verify whether the prepares match the pre-prepare by checking that they have the same view, sequence number and digest.

the pre-prepare and prepair phases of the algorithm guarantte that non-faulty replicas agree on a total order for the requests within a view. 

replica i multicast a <commit, v, n, d(m),i>sign to the other replics when prepared becomes true

we define the comitted and committed local predicates as follow: 

it ensures that any request that commit locally at a non-fauty replic will commit at f+ 1 or more non faulty replicas eventually. 


### garbage colleciont.
replica can discard message if they are are the reqest have been executed by at least f+1 non faulty replicas and it can prove this to others in view change
replica also need some proof that the state is correct

checkpoint with a proof is a stable checkpoint

the last stable checkpiont, zeor or more checkpoints that are not stable and a current state. 

<checkpoint, n,d,i> sign. A checkpoint with a proof becomes stable and the replica discards all pre-prepare, prepare and comit meessage. 

computing the proof is efficient becuase the digest can be computed using **incremental cryptogrpahy**

### view change
 the view change protocal provides liveness by allowing the system to make progress when the primary fails. View changes are triggered by timeout that prevent backups from wiating indefinitely for requests to execute. 
 
 







