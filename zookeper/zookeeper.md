# Zookeeper

## Zookeeper Atomic Broadcast Protocol

### Zookeeper vs Raft

- Both use a leader-based approach
- Both use a two-phase commit protocol:
  - Leader sends a `prepare` message to followers, followers respond with `ack` message, leader sends `commit` message
- Both grant totat order of committed transactions

Differences:

- Raft uses a randomized election timeout to avoid split votes and a heartbeat mechanism to avoid unnecessary elections.
- Zookeeper doesn't specifies an Heartbeat protocol, a leader is considered failed when it looses connection with a majority of followers, i.e. looses quorum.
- When a new leader is elected, Zookeeper requires that the new leader completes the previous leader's transactions before starting to process new transactions.Raft doesn't have this requirement.

[Zookeeper Atomic Broadcast documentation](./ZAB.pdf)
[Raft documentation](./raft.pdf)

### Zookeeper vs 2PC

- Both use a two-phase commit protocol, but 2PC is stronger and not partition-tolerant.
- 2PC requires all nodes to agree to a commit, while Zookeeper only requires a quorum. After a `commit` operation, 2PC requires a further `ack` from all nodes:

**2PC:**

```text
Coordinator                                          Participant
                             QUERY TO COMMIT
                 -------------------------------->
                             VOTE YES/NO             prepare*/abort*
                 <-------------------------------
commit*/abort*               COMMIT/ROLLBACK
                 -------------------------------->
                             ACKNOWLEDGEMENT          commit*/abort*
                 <--------------------------------  
end
```

**ZAB:**

```text
Coordinator                                          Participant
                             QUERY TO COMMIT
                 -------------------------------->
                             ACKNOWLEDGEMENT             prepare
                 <-------------------------------
commit                       COMMIT                      
                 -------------------------------->      
                                                         stores        
end
```
[Wikiperdia 2PC page](https://en.wikipedia.org/wiki/Two-phase_commit_protocol)
