# QoS basis and Procols

With Tcp/IP transport the entities communicate using resources available during execution dynamically **without predefined commitment**.

## Application Classification

- **Elastic:** traditional. Do not present quality constratints
  - Work betterwith low delays and work worst during congestions
  - Interactive with delays less than 200ms
- **Non Elastic**: time constrained modern application
  - Have time constraints, less tolerant. Can not work if are not present enough resources.
  - The service can be adaptive and compromise:
    - Delay adaptive -> audio drop packets
    - bandwidth adaptive -> video that adapt quality

![classification](./qos1.png)

## Stream qos parameters

Functional parameters:

- Promptness in reply
- Bandwidth: quantity of data transmitted bya channel with success per time unit
- Throughput
- Reliability: percentage of success/failures
- **Latency**: the time spent to send an information unit (bit) also meauserd as the round trip time (back and forth). $T_l = T_p + T_x + T_q$
  - $T_p$ : depends on the speed on light inside the medium **(space/speed)**
  - $T_x$ : depends in messages and bandwidth **(dimension/bandwidth)**
  - $T_q$ : delays on intermediate points. **Critical, involves all waiting overhead**
- **Jitter**: variance of the latency. In optimal situation latency is stable.
- **Skew**: Offset between multiple flows composing a unique stream

But also non functional parameters are important.

![jitter](./qos2.png)

A good service requires to identify bottlenecks and must consider resource management

- If I send 1 byte: latency is more perceived
- If I send many megabytes: bandwidth is more perceived

**Resource occupation:** product of $latency \times bandwith$

Simple strategies always applied:

- keep pipes full, but time must be considered carefully
- Buffering time inside applciations in typically considered

### Non functional parameters

Evaluated by the final users:

- Image details
- Image accuracy
- responde time in variation
- audio/video synchronization

Qos can be guaranteed only through a negotiated and controlled contract and after provisioning. **Necessity of observation and feedback: monitoring**.

### Quality of experience

- Relevance (priority)
- QoS perceived
- Cost
- security (integrity, authentification)

## Management

Management actions can be both:

- Proactive (static): decided before execution
- Reactive (dynamic): identified during distribution

Focus on **Service Level Agreement**

### Static

- Precise definition of Qos Levels and SLA
- Negotiation
- Admission control: comparison between requested Qos and available resources
- **reservation of resources**: needed resource definition for allocation to obtaion requested QoS
SLA represents the static agreement

### Dynamic

Monitoring of eventaul changes to respect the defined policy

- continuous measurement
- respect synchronization
- renegotiation of necessary resources
- **change of resources** to maintain Qos and adjustment to new situations

**Reservation and change** of resources **are local actions non provided in protocols**

### Active Path

You must find and active path between emitter and receiver, anyway, also by flooding

![active path](./qos3.png)

Among severals must choose one to employ. Intermediate nodes must agree on the negotiated SLA.

The specification of an SLA is always a coordinated eddort among emitters and receivers.

- Emitter: Can i send that specific streaming with that B bandwidth and that A accuracy?
- Receiver: I can accept just that streaming with that B1 bandwidth and that A accuracy, with latency L.

## Management and monitoring

Necessity to match application plane with strategies for efficiency control. Keep in mind the three planes of a service:

- User plane
- Management Plane: must collect data
- Control Plane: takes action based on data

Granting the QoS is high cost. Dynamic data collection mechanism must require not too many resources. **Minum intrusion principle:** monitoring must attempt not to comete with applications for resources.

### OSI System management

Osi distributed management: Use of **standard description of objects and actions**.

Defines a layer 7 protocol: CMIP (Common Management Information Protocol), which is an implementation for the CMIS (Common Management Information Service), allowing communication between network management applications and management agents. [1](https://en.wikipedia.org/wiki/Common_Management_Information_Protocol)

## Agent/Manager model

Management standard based on two roles:

- manager
- agents: responsible of managed resources

Manager/Agents model can lead to very simple implementation.

![Manager/agents model](./qos4.png)

### SNMP

Snmp: Simple Network Management Protocol, standard by IETF. It uses TCP/IP, very spread in UNIX systems and LAN environments, SNMP operates on CMIP subset (incompatiblewith CMIP standards). Since it was really simple, over the years had passed redefinitions to keep count of:

- security
- flexible management
- existing legacy systems
- not only devices, entities of any type

SNMP uses **one manager (only one)** and **some agents** that control variables representing the objects, identified by **unique names (OID in hierarchical directories)**, stored in the MIB database.

Manager requests actions `(get and set)` and receives response. Agents wait requests and can also send `trap`.

![snmp model](./snmp.png)

Some messages used on UDP (port 161 messages, port 162 inside manager for trap):

- `Set`
- `Get`
- `Get_Next` (multiple attribute)
- `Trap`

![snmp messages](./snmp1.png)

#### SNMP Agents

Must handle requests of get and set form the manager, can generate traps when some events occur.
![snamp agent](./snmp2.png)

#### SNMP Manager

Must handles respondes and traps from agents.

![snmp manager](./snmp3.png)

#### SNMP Standards

Data description ruled by:

- SMI (structure of management information), to define objects - ASN.1 and BER standard
- MIB (management of information base), to manage direcories of objects - X.500 standard

#### Proxy Agents

A proxy agent is an agent and a manager. Introduced to overcome the problem called **micro management** (i.e., the congestion around manager). The proxy can collect results and send them in an aggregated form.

![snmp proxies](./snmp4.png)

## Network and traffic Management

Snmp can deal only with variables inside the agents, the processes. To manage the network enter **Remote MONitor, RMON**, designed to give visibility on traffic.

It introduces monitor agents and and the interaction protocol between manager and monitors.

RMON is **oriented toward traffic and bandwidth**, not toward devices.

**Probe** is an entity capable of **monitoring packets** on the network, **can work autonomously** and **also disconnected from the manager** to track subsystems and **report filtered information** to the manager.

![rmon probes](./rmon.png)

Enhance model of distributed management based on:
| | |
| - | - |
| Active entities | Manager |
| Managed entities | Objects |
| Intermediate entities | Agents |

With objects that can be managers themself to organize a flexible hierarchy.

![rmon](./rmon1.png)

### Advanced network management

Managed object are the resources, described as objects:

- simple resources (a modem)
- complex resources (more interconnected systems)
- can be created dynamically

The managers realize management policies based on managing different agents of other managers (?). A manager can both insert a resource and remove it dynamically.

The agent can also execute actions on manager request.

#### CMISE/P

It's an advanced protocol used from telcom to manage WAN. It realizes a dynamic model at its maximal degree: it can also define new action for the agents dynamically.

## Control Plane Protocol: Session Protocol

SIP, session initiation protocol, is able to support and manage multimedia sessions. The goal is to define and manage a session to support a multimedia service that is provided by other protocols. SIP uses Http compatible content, text-based and purely client/server.

Messages:

![sip messages](./sip.png)

An interaction flow:

![interaction flow](./sip2.png)

There are different fucntional entities:

- User agent: endpoints, client and servers.
- Proxy server: routers at application level, can keep state of session transactions
- Redirect server
- registar server: service for user registration to the infrastructure
- Location server: service to allow link interested users to their location

![sip scenario](./sip3.png)

Messages are structured as: start-line, header, message body (optional). If it is a request in the startline there's the name-method, protocol version and the request URI. If it is a REPLY there's protocol version, state code, explicative phrase. the body can contain firther information on flow and service.

## Router Internet

### Best Effort

The routers move packets without differentiating queuing or scheduling and without distinguish between flows.

A router executes for **every packet** that is put into a **FIFO queue**

1. verification destination
2. check routing tables
3. select best output path (maximum match length)
4. Forward the packet

![standard router model](./routing.png)

Standard routers don't support any QoS.

![internal model of a router](./routing1.png)

The normal standard policy is FIFO, a unique queue for every flow of the router: no service differentiation. *Routers must pass messages ASAP.*

**Law of conservation of work (Kleinrock):** A router (work conservative) cannot be idle if there are packets on any input/output port (delays can't be introduced).

If there are $n$ flows with **$\lambda_n$ traffic** for every flow, and a **service mean time $\mu_n$**, then the **use is $\rho_n=\lambda_n\mu_n$**. Where:

- $\rho_n$ represents the mean usage
- $q_n$ represents the mean waiting time

The **Kleinrock Law** for work consertative scheduler states: **$\sum \rho_n q_n = C$ constant**.

If the router gives a lower delay/higher bandwidth to a flow, has to increase/reduce it for another one. Cannot favor a flow without damaging another one.

### Router QoS policies

Qos can be obtained by conditioning the traffic: monitoring of the traffic and taking actions to decide more sophisticated policies of services. Cloud be:

- delay some packets
- discard some packets

![qos router](./routing2.png)

QoS Routers have policies for queuing and scheduling, based on packet, length or destination/source (flows). A router can have a packet classifier and a function for traffic conditioning.

![traffic conditioner](./routing3.png)

How to intervene on routing to obtain respect of some SLAs? By marking every datagram and acting on it.

Traffic management is typically provided by intermediate router nodes. They must consider different flows to maintain the QoS:

- Routers must keep the **State to differentiate flows**
- Routers must make **queue managements activities**

### Models for QoS routers

QoS routers are non Internet complaint because **they don't respect the Kleinrock Law**.

- Leaky Bucket: the router has a limited memory for messages and some limits in output
- Token Bucket: the router is only delaying messages, that must kept into the memory waiting to get out.

The bucket models: control flows through capacity. **The policy is still best-effort**:

- If packets arrive to quickly they are dalayed
- If packets arrivie in execess they are thrown away or lost.

#### Leaky Bucket

The router must know the flows and possible service capacity. The router ACTIVELY shpaes services with the strategy of limiting output flows.(A bucket per flow).
![leaky bucket](./routing4.png)

Leaky bucket aims at **switching off packets bursts**. A packet is queued only there's space in the bucket. The packets can exit with a maximum speed $R$ that limits the allowed input flow $r,(r>R)$

#### Token Bucket

Token bucket considers flows history via tokens as authorization for passing.

- Empty bucket: not pass and wait
- Full bucket: tokens can be associated with packets to pass
- Partially full: some messages can pass, others have to wait

![token bucket](./routing5.png)

Token bucket allows packets bursts: if data arrives too quickly, beyond the admissible output flow, **they can exit when enough corresponding tokens are accumulated.**

![token production](./routing6.png)

The token imposes delay to **grant a requested buffered resources**.

![token effects](./routing7.png)

Often the two buckets are used in serial chain.

### Policies of QoS routers

While Internet routers are FIFO, respecting Kleinrock Law, **Qos Routers** don't: they **must penalize someone to give priority**. How?

Scheduling and queueing must respect properties:

- Implementation ease
- Fairness: any flow must be penalized the same as any other

#### General fair strategy: MAX-MIN

Max-Min share: requests of different resources by different flows must be considered in order of growing requests. (First the ones that require less)

- $C$ global max capacity of resources
- $X_n$ resources request by flow-n
- $m_n$ previously allocated resources to flow-n
- $M_n$ available resources for flow-n

![policies max-min](./policies.png)

The Max-Min model lets pass first the flow that has fewer demanding requests. the scaling down is done only in lack of resources.

#### Generalized processor scheduling

Defined some classes for packets (flows), each flows is configured with one weight $w_i$. The GPS ensures that the flow $i$ in the interval $(s,t]$, if the queue is never empty, then this relation is always true: $w_j O_i(s,t) >= w_i O_j(s,t)$.
> $O_k(s,t)$ is the amount of bits for the flow $k$ on the interval $(s,t]$.

This means that the flow $i$ receives at least the rate:

  $R_i= \frac{w_i}{\sum w_j}R$

It is a very fair policy: given a priority, at the worst case it is respected for each flow, if a flow has an empty queue then the others get is share in an equal way.

> :warning: it is not possible to implement GPS in reality, it is possible only to servce packets, not bits.

#### Round robin policies

Scheduling with priorities: easy to implement, can cause starvation of low priority flows. We split the band in fractions to prevent starvation.

![scheduling with priority](./queues.png)

Round robin visits each flow cyclically and send one packet per flow. If the queue is empty it can't skip it. (nda: on slides it's said that it cloud skip it, but at Lectures we said it clound't skip, idk)

**Weighted Round Robin:** Every queue is visited for every round a number of times equal to the weight. Works worst in vase of short flows.

**Deficit Round Robin:**

- Every flow has a state value (deficit 0).
- When arriving the top of the queue, the packet is extracted if it is less than a threshold length, otherwise it waits for a number of rounds associated with its length compared with the deficit threshold (by augmenting the deficit fo a specificamount each visit).
- the packets beyond the threshold waits an appropriate number of turns, proportional to their length.
Works well when there are a limited number of flows and with small packets on average.

#### Fair Queuing and its variations

GPS principle, as it were per-bit: A packet of a size N in a flow can be output only after visiting all other queues N times, by examining all flows 1 bit at time. If examining 1 bit at time we reach an end of packet, then it is extracted from the queue.

**FAIR QUEUING**: is the more suitable policy and simple to implement, and it is typically available on all routers even low-cost. The packets firsts to exit are the ones "that ends first". *Example with flows all the same weight:*
![GPS with packets](./queues1.png)

Fair queuing is a fair policy that also favors an optimal usage of router resources.

### Congestion Prevention

The main problem with QoS routers is **not knowing the incoming traffic in advance**. Solution: when possible, we insert the state inside the system to forecast the flow arrival.

One of the most unpleasant situation in best-effort systems is congestion. Often dealt with simple and reactive policies:

- Discard only excess packets (silently)
- Send indication to limit traffic (choke packets)

In QoS internet is possible to undertake congestion with preventive actions.

#### Proactive Policies Red

**Random Early Detection**: a queue for every flow with equal priority.

Congestion prevention with a random discarding of packets in every queue, even much earlier than the congestion situation.

RED defines the min, max and avarage length of the queue. In the lenght is under the min takes no action, if it's over the max discards all the new packets, otherwise **discards with probability proportional to queue length**.

The RED policy is successful preventing congestion.

## Internet Services and new Requirements

**Best effort**: no guaranteed throughput, possible delays, no duplication control or action to order guaratees. Good for elastic services like Internet.

**Controlled Load**: similar to best effort with low load, limitations on delay, **elastic services and tolerant real-time**.

**Guaranteed Load**: tight limit to delay, maximum guarantee on flows, real-time non tolerant services.

### Real Time streaming protocol

Web-based streaming transported up to a final client, **Real-player**.

The player commincates with the server via UDP or TCP, trying to obtain a better provisioning and adaptation, by exploitating only a **local receiver buffer strategy.**

The receiver does not wait the entire file to provide the stream, but assumes and keeps a receiver buffer to always contain some frames.

### Internet transition

If tcp is elastic with ordering warranties and flow control, OSI asks QoS at any level. Warranties as a cost. Internet is going from a low-cost low-performance to differentiated cost and performances with 2 suites:

- Integrated Services: working at level 7 and at single flow level
- Differentiated Services: joining and classifying flows under different qualities, level 3

## IntServ: Integrated services

New protocols to adapt the Internet to obtain a better control on operations and resources, keeping compatibility with IP best-effort policies.

The analysis is done per flow and per hop, without considering scalability too much:

- RSVP: resource reservation setup protocol, signaling, to plan resources
- RTCP: real-time control protocol, dynamic management, keep negotiated QoS
- RIP: real-time protocol, General operational messages (UDP)

IntServ must work during both static and dynamic phases to gran QoS. The goal is to produce an active path, connecting the sender and the receiver for the whole flow.

The static actions (out of band) can be expensive, the dynamic actions are time critical.

### RSVP: reservation protocol

- the sender signals itself to the receiver "I'm here", and propose a path
- the receiver answers back, to find out the best path and book the needed resources on the nodes.
How to propagate the messages is not part of the protocol. The sender keeps singling itself, even if no one is interested.

RSVP enables resource reservation in the opposite verse. Of course Rsvp uses UDP.
The nodes all know their neighbor, they communicate to enavle the reservation of needed resources to guarantee an agreed SLA. **This protocol is before the real service flow and is out of band.**

The admitted state is soft and many optimization are possible (shared paths, multiple providers). the implementation is free to decide many decisions, and since it's out of band, also optimal.

![int serve rsvp](./intserv.png)

Summary:

- Single-hop protocol
- The only goal is to signal information to reserve necessary resources
- Produces state on every node of the path
- Sharing of active path in various forms.
- RSVP is not a routing protocol, but must be compatible with them (ipv4 and ipv6)


