# Pass-by-reference (&)
This one is just a reminder for myself since I always make the mistake of forgetting to pass-by-reference when I should (aside from for loops and functions, that's only time I remember!).

Passing by reference makes a HUGE optimization vs. forgetting and ending up passing by value and creating redunant copies. Especially when you're dealing with large objects, like: 
- std::vector<T>
- std::map<K, V>
- std::string (non-trivial size)
- Custom struct/class with many fields or dynamic allocations

## Exhibit A:
Here is code implementing a data structure that can efficiently manage data packets in a network router (no need to read the whole thing just focus on the arrowed lines of code):
```cpp
class Router {
private:
    
    struct PacketKey {
        int source;
        int destination;
        int timestamp;

        bool operator==(const PacketKey& other) const {
            return source == other.source && destination == other.destination && timestamp == other.timestamp;
        }
    };

    struct PacketKeyHash {
        size_t operator()(const PacketKey& key) const {
            size_t h1 = hash<int>{}(key.source);
            size_t h2 = hash<int>{}(key.destination);
            size_t h3 = hash<int>{}(key.timestamp);
            return h1 ^ (h2 << 1) ^ (h3 << 2);
        }        
    };

    unordered_set<PacketKey, PacketKeyHash> unique_packets;
    queue<PacketKey> q; 
    unordered_map<int, vector<int>> by_destination;
    int limit = 0;
    
public:
    Router(int memoryLimit) {
        limit = memoryLimit;
    }
    
    bool addPacket(int source, int destination, int timestamp) {
        
        PacketKey new_packet;
        new_packet.source = source;
        new_packet.destination = destination;
        new_packet.timestamp = timestamp;

        if (unique_packets.find(new_packet) != unique_packets.end()) { 
            return false;
        }

        if (q.size() == limit) { 
            PacketKey oldest_packet = q.front(); // <============================
            unique_packets.erase(oldest_packet);
            by_destination[oldest_packet.destination].erase(by_destination[oldest_packet.destination].begin()); 
            q.pop();
        }
        
        q.push(new_packet);
        unique_packets.insert(new_packet); 
        by_destination[destination].push_back(timestamp);
        return true;
    }
    
    vector<int> forwardPacket() {
        if (q.size() > 0) {
            PacketKey packet = q.front(); // <============================
            vector<int> packet_vec{packet.source, packet.destination, packet.timestamp};
            unique_packets.erase(packet);
            by_destination[packet.destination].erase(by_destination[packet.destination].begin()); 
            q.pop();
            return packet_vec;
        } else {
            return {};
        }
        
    }
    
    int getCount(int destination, int startTime, int endTime) {
        // Especially this one V
        vector<int> timestamps_with_destination = by_destination[destination]; // <=========================================================== 
        auto start_it = lower_bound(timestamps_with_destination.begin(), timestamps_with_destination.end(), startTime);
        auto end_it = upper_bound(timestamps_with_destination.begin(), timestamps_with_destination.end(), endTime);

        return distance(start_it, end_it);
    }
};
```
The arrows show a couple of situations where I could've passed by reference but forgot, meaning by default, I created a copy of the object and passed it to a new variable. 

The one in particular I want to mention here is this line of code:
```
vector<int> timestamps_with_destination = by_destination[destination];
```
In the class, I defined a hashmap with an `int` key associated with `vector<int>` values. For the `getCount()` method, I want to grab
a specific `vector<int>` based on a given `destination` (which is an int). By forgetting one little ampersand, it costs me huge amounts of memory when the code is tested on very large testcases.

The correct thing to do here is: ` vector<int>& timestamps_with_destination = by_destination[destination];`

I could even take it a step further--since I'm passing by reference, meaning I now have access to/can modify the actual vector stored in the map, I would want to add a `const` keyword in front of `vector<int>&` to ensure it stays unchanged. Like this:
```
const vector<int>& timestamps_with_destination = by_destination[destination];
```
Because the variable `timestamps_with_destination`'s purpose in the function isn't to modify anything or be modified. So I can promise that with the `const` keyword.
