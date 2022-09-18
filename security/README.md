Security Test
======

The security test belongs to non-functional test. It is related to system security. Its primary function is to find the vulnerabilities in the test and avoid the hacking problem.

There are two obvious tests which are fuzz test and penetration test. This kind of tests can be provided by the language itself. The go language has fuzz test in the language. 

wfuzz test is written in Python language. It can solve the attacks like parameter, authentication, header and form. wfuzz is module and plugin system. The attack validation can be finished by plugin. The scanner is used to find the potential vulnerabilities. The wfuzz is also wrapped into docker image. The container can be used to perform the fuzz test work. The source code is located at https://github.com/xmendez/wfuzz

The network packet can be captured and analized to do security check. The wireshark and tcpdump is two useful tool. The injection is one way to attack the system and destory the system by its vulnerabilities. The parameter and form injection is the approach to find the http and interface vulnerabilities. The certificate and SSL/TLS are one aspect of security concern. 

