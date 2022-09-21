# Nginx

## Request Limit

In the first part of source code, the data types and function prototypes are defined. The function implementations are declared in the following part. The functions are grouped into command list. The type variables are instantialized by literal statement.The request limit handler is created to enforce the request limit. The input parameter is http request type. The http request is standed by a struct type. 

We should think how the object are represented by data structure. The definition of data structure means one type in the digital world. The nginx has a set of type system. The memory is allocated in the unit of pool. The data types of network are defined in the network interface file. The data and its operations are defined in the same source code file. 

## Connection Limit

