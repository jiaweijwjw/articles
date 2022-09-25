# How DNS Works

All devices on the internet communicate with each other via IP addresses (**e.g 192.168.2.3**) which are numbers, instead of the human readable words in a domain name (**e.g www.domain_name.com**).

However, whenever you want to visit a website, you just type in the URL of the site instead of directly entering the IP address of the site as it is easier for us humans to remember words than number combinations. Your browser will then try to connect you to the server of the website that you want to visit and display the fetched data on your screen.

The service which allows you to connect to a server using the domain name instead of the IP address is called the DNS. 

## Technical explanation

Let us call the browser of the user the "client" and the host of the website the "server". For us to establish a HTTP connection between the client and the server, the client must first know the IP address of the server.

1. When the user inputs the domain name of the server, the client will first check in its own cache if there is any remnants of what it remembers about this domain name. (whether the browser has already remembered the IP address of this domain name previously)

   * If the browser does not know the mapping (first time the user visits this website or been a long time since he has last visited it), then it will ask for the help of the DNS.

There are 4 main nodes which are responsible for the DNS resolution.
* DNS resolver
* root name server
* TLD (top level domain) name server
* authoritative name server

The client will only communicate with the DNS resolver and this node will help us to contact the remaining nodes during the resolution process. The DNS resolver is usually located near to the end users (provided by ISPs or some organizations / universities have their own DNS servers.)

2. If the client is lucky and the website that the user wants to connect to already has a mapping on this node (a mapping of this domain name and the respective IP address), the DNS resolver can immediately return the IP address and then the client can start making a connection with the server. 

   * If the domain is not already cached, then this DNS resolver will go and query the remaining nodes. The query goes through a hierarchical system. In most cases, iterative query is used so we will focus on that.  
 
3. The DNS resolver will go and query the root server first. However, the root server will not return the IP address of the server directly. Instead, it will return the IP address of the TLD name server which is the next node that the DNS resolver must contact.
   * For example, if the user is searching for www.google.com, then the root server will point the resolver to the TLD DNS server which handles the .com top level domain (.com, .org, .net etc).

4. The DNS resolver will then query this TLD name server and again, the TLD DNS server will not return use the IP address of the server directly. Instead, it will return the IP address of the authoritative name server which handles the domain name which the client is looking for. The authoritative name server is handled by the organization itself.
   * For example, if the user is searching for www.google.com, then the authoritative name server will be one of google's DNS server.

5. The DNS resolver will then finally query this authoritative name server to retrieve the actual IP address of the server which the client wants to connect to.


### Additional Notes

* At any one point in time, if any of the nodes (DNS servers) above already have the required mapping being cached, then it can provide the resolver with the information without going through the entire hierarchy. 

* On the user's computer itself there is also a DNS cache which is actually the first step after checking the browser cache. If this cache does not contain a mapping of the hostname you are looking for, then it will pass to the DNS resolver (provided by ISPs) as mentioned above)

* On this local DNS and the ones provided by the ISPs, the records are cached, meaning they have a TTL (Time to live). On the actual hierarchy, meaning the rootserver, TLD and authoritative nameserver, the mappings are actual records (like the A, NS, CNAME records etc.), not cache.

* for the recursive resolution, it will put more work on the DNS servers, whereas the iterative one will just put the work on the resolver.

![dns resolution process](../images/dns_recursive_vs_iterative.png "DNS resolution process")




## Analogy

Now let me tell a short story to explain the resolution process in simple terms.

* One day, you are walking on the street and you found that someone has lost his student card. The only information written on the student card is the person's name and the university he is attending. The goal is to call the person on his mobile number to inform him of his lost card.
* The first "cache" which you will check is your own brain. If you know the person on the student card personally and you remember his mobile number, then you can call him directly.
* The next "cache" that you will check for is your mobile phone. You may not remember the person off your head but you could have collaborated with him before and thus have his number saved on your phone.
* If the above "caches" were both a miss, then you have no clue about how to contact this person. However, you are lucky that you have a mobile phone, which will act as your resolver. You can use the mobile phone to make severals calls to several people to find out the contact number of the student.
* since you know the university that the student is studying in, you will first give a call to the university main office. However, the university does not have the contact number of the student. But it knows the faculty that the student is in. Hence, the university office will provide you with the contact number to the faculty that the student is in.
* Let us assume that the student is in the engineering faculty and you have made the second call to the faculty's office. However, the faculty also does not have the contact number of the student. However, it knows the class that the student is in. Hence, the faculty office will provide you with the contact number to the teacher teaching the class that the student is in.
* Finally, you call the teacher to request for the contact number of the student and the teacher gives it to you.
* You can make the call to the student to inform him of his lost card. You then hang up the call.
* The call log on your phone will be deleted after some time. However, lets say that you forgot to inform the student the meeting place where you will be passing him his student card and you want to call him again. This time, you can directly make the call to the student. If the call log to the student's mobile number has been deleted, you then try to see what is the contact 'nearest' to the student that is still saved in your call log. (e.g if the teacher's mobile number is still in your call log, you can just call the teacher to ask for the student's number again.)

