# Jmeter Usage

## Installation

```sh

wget https://dlcdn.apache.org//jmeter/binaries/apache-jmeter-5.5.tgz

tar xvf apache-jmeter-5.5.tgz

export PATH=$PATH:/root/apache-jmeter-5.5/bin

```

## Test Plan

There is one next is null method in the loop controller. The initialize step is rerun and next method is executed. The running version is recoved in the initialization step. In the recover method, the interator is got from property map. The property map is iterated by the iterator. The property value is got from the map collection. The current element is the pointer. If the current element is null, next is null method is executed and new cycle is started. The main purpose of controller is to find next sampler. The controller is one kind of teset elements. 

## Version 3.3

The function support is added in this release.

## Debug Tips

There are several ways to debug jmeter scripts. One is to use debug sampler. The second is view result tree. The third is log viewer. The dummy sampler is last approach. 