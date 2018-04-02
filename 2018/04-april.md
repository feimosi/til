<h1 align="center">02.04.2018</h1>

## Hot vs Cold observables

User interface events like button clicks, mouse movement, etc. are hot. 
They will always push even if we're not specifically reacting to them with a subscription. 
The window resize event is a hot observable: the resize$ observable fires whether or not subscription exists.

A cold observable begins pushing only when we subscribe to it. If we subscribe again, it will start over.
