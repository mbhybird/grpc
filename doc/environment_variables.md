gRPC environment variables
--------------------------

gRPC C core based implementations (those contained in this repository) expose
some configuration as environment variables that can be set.

* http_proxy
  The URI of the proxy to use for HTTP CONNECT support.

* GRPC_ABORT_ON_LEAKS
  A debugging aid to cause a call to abort() when gRPC objects are leaked past
  grpc_shutdown(). Set to 1 to cause the abort, if unset or 0 it does not
  abort the process.

* GOOGLE_APPLICATION_CREDENTIALS
  The path to find the credentials to use when Google credentials are created

* GRPC_SSL_CIPHER_SUITES
  A colon separated list of cipher suites to use with OpenSSL
  Defaults to:
    ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES256-GCM-SHA384

* GRPC_DEFAULT_SSL_ROOTS_FILE_PATH
  PEM file to load SSL roots from

* GRPC_POLL_STRATEGY [posix-style environments only]
  Declares which polling engines to try when starting gRPC.
  This is a comma-separated list of engines, which are tried in priority order
  first -> last.
  Available polling engines include:
  - epoll (linux-only) - a polling engine based around the epoll family of
    system calls
  - poll - a portable polling engine based around poll(), intended to be a
    fallback engine when nothing better exists
  - legacy - the (deprecated) original polling engine for gRPC

* GRPC_TRACE
  A comma separated list of tracers that provide additional insight into how
  gRPC C core is processing requests via debug logs. Available tracers include:
  - api - traces api calls to the C core
  - bdp_estimator - traces behavior of bdp estimation logic
  - call_error - traces the possible errors contributing to final call status
  - channel - traces operations on the C core channel stack
  - client_channel - traces client channel activity, including resolver
    and load balancing policy interaction
  - combiner - traces combiner lock state
  - compression - traces compression operations
  - connectivity_state - traces connectivity state changes to channels
  - channel_stack_builder - traces information about channel stacks being built
  - http - traces state in the http2 transport engine
  - http1 - traces HTTP/1.x operations performed by gRPC
  - inproc - traces the in-process transport
  - flowctl - traces http2 flow control
  - op_failure - traces error information when failure is pushed onto a
    completion queue
  - round_robin - traces the round_robin load balancing policy
  - pick_first - traces the pick first load balancing policy
  - resource_quota - trace resource quota objects internals
  - glb - traces the grpclb load balancer
  - queue_pluck
  - queue_timeout
  - server_channel - lightweight trace of significant server channel events
  - secure_endpoint - traces bytes flowing through encrypted channels
  - timer - timers (alarms) in the grpc internals
  - timer_check - more detailed trace of timer logic in grpc internals
  - transport_security - traces metadata about secure channel establishment
  - tcp - traces bytes in and out of a channel
  - tsi - traces tsi transport security

  The following tracers will only run in binaries built in DEBUG mode. This is
  accomplished by invoking `CONFIG=dbg make <target>`
  - metadata - tracks creation and mutation of metadata
  - closure - tracks closure creation, scheduling, and completion
  - pending_tags - traces still-in-progress tags on completion queues
  - polling - traces the selected polling engine
  - queue_refcount
  - error_refcount
  - stream_refcount
  - workqueue_refcount
  - fd_refcount
  - cq_refcount
  - auth_context_refcount
  - security_connector_refcount
  - resolver_refcount
  - lb_policy_refcount
  - chttp2_refcount

  'all' can additionally be used to turn all traces on.
  Individual traces can be disabled by prefixing them with '-'.

  'refcount' will turn on all of the tracers for refcount debugging.

  if 'list_tracers' is present, then all of the available tracers will be
  printed when the program starts up.

  Example:
  export GRPC_TRACE=all,-pending_tags

* GRPC_VERBOSITY
  Default gRPC logging verbosity - one of:
  - DEBUG - log all gRPC messages
  - INFO - log INFO and ERROR message
  - ERROR - log only errors

* GRPC_DNS_RESOLVER
  Declares which DNS resolver to use. The default is ares if gRPC is built with
  c-ares support. Otherwise, the value of this environment variable is ignored.
  Available DNS resolver include:
  - native (default)- a DNS resolver based around getaddrinfo(), creates a new thread to
    perform name resolution
  - ares - a DNS resolver based around the c-ares library
