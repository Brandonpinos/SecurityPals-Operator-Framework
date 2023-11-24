# Understanding Operators

The motivation behind creating this file is to help others understand what Kubernetes Operators are, in a simple and concise language.


The descriptions are borrowed from [Techworld by Nana](https://www.youtube.com/watch?v=ha3LjlD6g7g), you could use this file as a readalong to the video to easily understand operators.


## Introduction to Stateful vs Stateless Applications


The entire point of operators come in when managing stateful applications in Kubernetes. Hence, first, we ought to understand what are stateful and stateless applications.


A **stateless application** is an application program that does not save client data generated in one session for use in the next session with that client. Every interaction with a stateless application is regarded as independent, and the application has no memory of previous interactions. A stateless system sends a request to the server and relays the response (or the state) back without storing any information. On the other hand, stateful systems expect a response, track information, and resend the request if no response is received.


On the other hand, a **stateful application** is one that saves data about each client session and uses that data the next time the client makes a request. Stateful applications track information about the state of a connection or application. They can be returned to again and again, like online banking or email. They're performed with the context of previous transactions and the current transaction may be affected by what happened during previous transactions. For these reasons, stateful apps use the same servers each time they process a request from a user. If a stateful transaction is interrupted, the context and history have been stored so you can more or less pick up where you left off.


From a DevOps perspective, a stateful application will be more complex to deploy and maintain because you need to provide each instance with access to a persistent data store. Stateless applications are fully decoupled from their environment which typically makes them easier to containerize and scale in the cloud.

Sources:
- [10 Key Differences Between Stateful and Stateless - Spiceworks.](https://www.spiceworks.com/tech/cloud/articles/stateful-vs-stateless/)
- [Stateless Over Stateful Applications | by Rachna Singhal - Medium.](https://medium.com/@rachna3singhal/stateless-over-stateful-applications-73cbe025f07)
- [Stateful vs stateless - Red Hat.](https://www.redhat.com/en/topics/cloud-native-apps/stateful-vs-stateless.)
- [AWS Applications - Stateful vs. Stateless.](https://digitalcloud.training/stateful-vs-stateless-aws-applications/)
- [Stateful vs Stateless Applications: How They Impact DevOps - How-To Geek.](https://www.howtogeek.com/devops/stateful-vs-stateless-applications-how-they-impact-devops/)


### Managing Stateless vs Stateful Applications in Kubernetes without Operators


Kubernetes has a control loop that automatically recreates a pod when it dies or is updated, and by default, it uses the updated image version or restores the original image. This is also a relevant consideration when scaling a Kubernetes application. 


This is fine for *stateless applications*, as previous client interactions or operational states are not relevant for the current state of the application. 


However, for *stateful* applications, this is a bit more complicated. If one needs to preserve a client state for a particular pod, essentially all of the pods could be different. Therefore, the pods need to be updated and scaled in a careful synchronized manner.


The problem is, there is no standard solution for all of this, given MySQL has its own way of updating things, Postgresql and Elasticsearch will have their own methods. This means, there needs to be a human *operator*, who has to synchronize all of these updates and pod replications to maintain the operations of the application. That, essentially, beats the entire purpose of using Kubernetes, which is supposed to automate the scaling process of application deployments.



