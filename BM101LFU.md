```java
import java.util.*;
 
class Node{
    int key;
    int val;
    int freq;
    public Node(){}
    public Node(int key,int val,int freq){
        this.key=key;
        this.val=val;
        this.freq=freq;
    }
}
public class Solution {
    int k;
    int[][] operators;
    int minfreq=0;
    HashMap<Integer,Node> key_map=new HashMap<>();
    HashMap<Integer,LinkedList<Node>> freq_map=new HashMap<>();
    public int[] LFU (int[][] operators, int k) {
       this.k=k;
       this.operators=operators;
       List<Integer> list=new ArrayList<>();
       for(int[] op:operators){
           if(op[0]==1) set(op[1],op[2]);
           else if(op[0]==2) {
               int val=get(op[1]);
               list.add(val);
           }
       }
       int[] res=new int[list.size()];
       for(int i=0;i<list.size();i++) res[i]=list.get(i);
       return res;
    }
    private void set(int key,int value){
        if(key_map.containsKey(key)){
            Node node=key_map.get(key);
            int freq=node.freq;
            freq_map.get(freq).remove(node);
            key_map.remove(node.key);
            if(freq_map.get(freq).size()==0){
                freq_map.remove(freq);
                if(minfreq==freq) minfreq++;
            }
            LinkedList<Node> list=freq_map.getOrDefault(freq+1,new LinkedList<>());
            list.addFirst(new Node(key,value,freq+1));
            freq_map.put(freq+1,list);
            key_map.put(key,freq_map.get(freq+1).peekFirst());
        }else{
            if(key_map.size()==k){
                Node node=freq_map.get(minfreq).peekLast();
                freq_map.get(minfreq).remove(node);
                key_map.remove(node.key);
                if(freq_map.get(minfreq).size()==0){
                    freq_map.remove(minfreq);
                }
            }
             LinkedList<Node> list=freq_map.getOrDefault(1,new LinkedList<>());
             list.addFirst(new Node(key,value,1));
             freq_map.put(1,list);
             key_map.put(key,freq_map.get(1).peekFirst());
             minfreq=1;
            }
    }
    private int get(int key){
        if(!key_map.containsKey(key)) return -1;
        Node node=key_map.get(key);
        int val=node.val;
        int freq=node.freq;
        freq_map.get(freq).remove(node);
        key_map.remove(node,key);
        if(freq_map.get(freq).size()==0){
            freq_map.remove(freq);
            if(minfreq==freq) minfreq++;
        }
        LinkedList<Node> list=freq_map.getOrDefault(freq+1,new LinkedList<>());
        list.addFirst(new Node(key,val,freq+1));
        freq_map.put(freq+1,list);
        key_map.put(key,freq_map.get(freq+1).peekFirst());
        return val;
    }
}
```

