Process
===

### 확인
```sql
SHOW PARAMETER PROCESSES;
/*
NAME                       TYPE     VALUE
-------------------------- -------- -------
aq_tm_processes            integer  1
db_writer_processes        integer  4
gcs_server_processes       integer  0
global_txn_processes       integer  1
job_queue_processes        integer  10
log_archive_max_processes  integer  4
processes                  integer  20000
*/

SELECT * FROM V$RESOURCE_LIMIT;
/*
RESOURCE_NAME	CURRENT_UTILIZATION	MAX_UTILIZATION	INITIAL_ALLOCATION	LIMIT_VALUE
processes	67	117	     20000	     20000
sessions	104	160	     30048	     30048
enqueue_locks	702	754	    350595	    350595
enqueue_resources	691	691	    142227	 UNLIMITED
ges_procs	0	0	         0	         0
ges_ress	0	0	         0	 UNLIMITED
ges_locks	0	0	         0	 UNLIMITED
ges_cache_ress	0	0	         0	 UNLIMITED
ges_reg_msgs	0	0	         0	 UNLIMITED
ges_big_msgs	0	0	         0	 UNLIMITED
ges_rsv_msgs	0	0	         0	         0
gcs_resources	0	0	 UNLIMITED	 UNLIMITED
gcs_shadows	0	0	 UNLIMITED	 UNLIMITED
smartio_overhead_memory	0	0	         0	 UNLIMITED
smartio_buffer_memory	0	0	         0	 UNLIMITED
smartio_metadata_memory	0	0	         0	 UNLIMITED
smartio_sessions	0	0	         0	 UNLIMITED
dml_locks	0	0	    132208	 UNLIMITED
temporary_table_locks	0	0	 UNLIMITED	 UNLIMITED
transactions	0	0	     33052	 UNLIMITED
branches	0	2	     33052	 UNLIMITED
cmtcallbk	0	2	     33052	 UNLIMITED
max_rollback_segments	0	0	     33052	     65535
sort_segment_locks	0	37	 UNLIMITED	 UNLIMITED
k2q_locks	0	0	     60096	 UNLIMITED
max_shared_servers	1	1	 UNLIMITED	 UNLIMITED
parallel_max_servers	0	8	        48	      3600
*/
```

<br>

### 참고
* [Session](../session/README.md)

<br>
