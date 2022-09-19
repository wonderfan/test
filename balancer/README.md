# Load Balancer

## Nginx

The configuration header file is created for the configuration process. The core functions are declared in the nginx core header file. The nginx header file is all about nginx process. The macro variables are defined in the header file. In the nginx implementation file, lots of function prototypes are defined. The array type variables are declared for debug point, command list, core modules. 

The procedures in the core file are debug intilization, time initialization, process id initialization, log initialization, ssl initialization, cycle pool initialization, module initialization, signal process registry, configuration file loading, process cycle.

The configuration file is a bit hard in the low level language. The file is opened, parsed and assigned into memory area. The whole capabilities are enabled and supported by the modules. The http proxy module and upstream module are used in the proxy module. 

The nginx cycle is defined by struct type. In the struct body, the type is before the variable name. The types for nginx cases are definied like list, array, str and int. 

As for the document and book, there are nginx cookbook, mastering nginx, nginx module reference on the internet. Each directive ends with a semicolon. The curly braces denotes a new configuration context. The use directive in the events section can choose the event mechanism. The default value is epool mechanism. The http configuration section controls all the aspects of working with http module. The http section includes io and sockect configurations. 

The proxy program typically uses linux fork model. A new process is forked for each connection. nginx process model is event-based model. The maximum number of open file descriptors is controlled by operating system. The operating system maintains a range of ports for outgoing connections. nginx serves as reverse proxy by termining client connection and opening new ones for upstream server. The proxy_pass directive fulfills this role. The upstream directives starts a new context which a group of upstream servers are defined. 


