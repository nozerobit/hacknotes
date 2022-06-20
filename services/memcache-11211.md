# Memcache - 11211

## Memcache Information

Memcached is a general-purpose distributed memory caching system \(pronounced mem-cashed, mem-cash-dee\). It's frequently used to speed up dynamic database-driven websites by caching data and objects in RAM to reduce the number of times an external data source \(such as a database or API\) needs to be accessed. ​

Default port: 11211

```text
PORT      STATE SERVICE
11211/tcp open  unknown
```

## Enumeration

### Manual

To exfiltrate all the data from a memcache instance, do the following:

* Look for slabs that have active things. 
* Get the slabs' key names that were detected previously. 
* Get the key names and exfiltrate the saved data.

Keep in mind that this is merely a cache, so data may appear and disappear.

```text
echo "version" | nc -vn -w 1 <IP> 11211      #Get version
echo "stats" | nc -vn -w 1 <IP> 11211        #Get status
echo "stats slabs" | nc -vn -w 1 <IP> 11211  #Get slabs
echo "stats items" | nc -vn -w 1 <IP> 11211  #Get items of slabs with info
echo "stats cachedump <number> 0" | nc -vn -w 1 <IP> 11211  #Get key names (the 0 is for unlimited output size)
echo "get <item_name>" | nc -vn -w 1 <IP> 11211  #Get saved info
​
#This php will just dump the keys, you need to use "get <item_name> later"
sudo apt-get install php-memcached
php -r '$c = new Memcached(); $c->addServer("localhost", 11211); var_dump( $c->getAllKeys() );'
```

### Alternative Manual Solution

```text
sudo apt install libmemcached-tools
memcstat --servers=127.0.0.1 #Get stats
memcdump --servers=127.0.0.1 #Get all items
memccat  --servers=127.0.0.1 <item1> <item2> <item3>
```

### Automatic

```text
nmap -n -sV --script memcached-info -p 11211 <IP>
```

