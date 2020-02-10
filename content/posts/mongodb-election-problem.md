---
title: "MongoDB选举机制问题"
date: 2020-02-07T19:21:49+08:00
draft: true
toc: false
images:
tags: 
  - MongoDB
---



主节点日志信息：
```bash
2020-02-07T11:01:54.298+0000 I  ELECTION [replexec-1504] Not starting an election, since we are not electable due to: Not standing for election because I cannot see a majority (mask 0x1)
2020-02-07T11:02:04.407+0000 I  ELECTION [replexec-1508] Starting an election, since we've seen no PRIMARY in the past 10000ms
2020-02-07T11:02:04.407+0000 I  ELECTION [replexec-1508] conducting a dry run election to see if we could be elected. current term: 205
2020-02-07T11:02:04.441+0000 I  ELECTION [replexec-1504] VoteRequester(term 205 dry run) failed to receive response from sh-2.xiexingchao.ink:27018: HostUnreachable: Error connecting to sh-2.xiexingchao.ink:27018 (101.132.125.136:27018) :: caused by :: Connection refused
2020-02-07T11:02:04.447+0000 I  ELECTION [replexec-1508] VoteRequester(term 205 dry run) received a yes vote from sz-1.xiexingchao.ink:27018; response message: { term: 205, voteGranted: true, reason: "", ok: 1.0 }
2020-02-07T11:02:04.452+0000 I  ELECTION [replexec-1504] VoteRequester(term 205 dry run) received a yes vote from sh-3.xiexingchao.ink:27018; response message: { term: 205, voteGranted: true, reason: "", ok: 1.0, $gleStats: { lastOpTime: Timestamp(0, 0), electionId: ObjectId('000000000000000000000000') }, lastCommittedOpTime: Timestamp(1579203217, 1), $configServerState: { opTime: { ts: Timestamp(1581073301, 1), t: 176 } }, $clusterTime: { clusterTime: Timestamp(1581073301, 1), signature: { hash: BinData(0, 0000000000000000000000000000000000000000), keyId: 0 } }, operationTime: Timestamp(1581000123, 1) }
2020-02-07T11:02:04.452+0000 I  ELECTION [replexec-1504] dry election run succeeded, running for election in term 206
2020-02-07T11:02:04.487+0000 I  ELECTION [replexec-1508] VoteRequester(term 206) failed to receive response from sh-2.xiexingchao.ink:27018: HostUnreachable: Error connecting to sh-2.xiexingchao.ink:27018 (101.132.125.136:27018) :: caused by :: Connection refused
2020-02-07T11:02:04.494+0000 I  ELECTION [replexec-1509] VoteRequester(term 206) received a yes vote from sz-1.xiexingchao.ink:27018; response message: { term: 206, voteGranted: true, reason: "", ok: 1.0 }
2020-02-07T11:02:04.497+0000 I  ELECTION [replexec-1504] VoteRequester(term 206) received a yes vote from sh-3.xiexingchao.ink:27018; response message: { term: 206, voteGranted: true, reason: "", ok: 1.0, $gleStats: { lastOpTime: Timestamp(0, 0), electionId: ObjectId('000000000000000000000000') }, lastCommittedOpTime: Timestamp(1579203217, 1), $configServerState: { opTime: { ts: Timestamp(1581073301, 1), t: 176 } }, $clusterTime: { clusterTime: Timestamp(1581073301, 1), signature: { hash: BinData(0, 0000000000000000000000000000000000000000), keyId: 0 } }, operationTime: Timestamp(1581000123, 1) }
2020-02-07T11:02:04.497+0000 I  ELECTION [replexec-1504] election succeeded, assuming primary role in term 206

```

Arbiter 日志信息：

```bash
2020-02-07T11:02:04.425+0000 I  ELECTION [conn20795] Received vote request: { replSetRequestVotes: 1, setName: "shardReplSet-1", dryRun: true, term: 205, candidateIndex: 3, configVersion: 9, lastCommittedOp: { ts: Timestamp(1581000123, 1), t: 205 } }
2020-02-07T11:02:04.425+0000 I  ELECTION [conn20795] Sending vote response: { term: 205, voteGranted: true, reason: "" }
2020-02-07T11:02:04.471+0000 I  ELECTION [conn20795] Received vote request: { replSetRequestVotes: 1, setName: "shardReplSet-1", dryRun: false, term: 206, candidateIndex: 3, configVersion: 9, lastCommittedOp: { ts: Timestamp(1581000123, 1), t: 205 } }
2020-02-07T11:02:04.471+0000 I  ELECTION [conn20795] Sending vote response: { term: 206, voteGranted: true, reason: "" }

```

参考：

+ https://blog.csdn.net/pengpengzhou/article/details/84344719
+ https://dba.stackexchange.com/a/46322
+ https://groups.google.com/d/msg/mongodb-user/-zEDz5X2oTc/tmlKYxtxDQAJ