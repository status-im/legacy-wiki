## General Idea

Today all connections of LES clients to their full nodes are treated the
same. There's no differentiation between their communication behaviour
but also no way of guaranteed service quality for the clients by the
full node service provider. So the general idea of the **LES Service
Model** is to separate between *free* and *paid services* in the future.
The first is a good indicator for high bandwidth and good service
quality but with the risk getting dropped in case of too many paying
clients demand their service. The latter instead is paying the full node
service provider by the client for a guaranteed bandwidth and service
quality at the free connections expense.

Beside the pure handling of subscriptions between clients and full node
service providers this also will lead to a larger distributed
infrastructure for a full node service provider market. Here different
providers can register their service and possibly advertise it together
with their different subscription models. It's getting even more
interesting when subscribers of services can review those so that other
clients can make their choices on the registered criteria as well as the
reviews.