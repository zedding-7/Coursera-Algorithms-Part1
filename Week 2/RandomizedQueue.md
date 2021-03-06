```java
import java.util.Iterator;
import edu.princeton.cs.algs4.StdRandom;
import java.util.NoSuchElementException;
public class RandomizedQueue<Item> implements Iterable<Item> {
    private Node first, last;
    private int size;

    private class Node {
        Item item;
        Node next;
        Node pre;
    }

    // construct an empty randomized queue
    public RandomizedQueue() { }

    // is the randomized queue empty?
    public boolean isEmpty() {
        return first == null || last == null;
    }

    // return the number of items on the randomized queue
    public int size() {
        return size;
    }

    // add the item
    public void enqueue(Item item) {
        if (item == null) {
            throw new IllegalArgumentException();
        }
        Node oldLast = last;
        last = new Node();
        last.item = item;
        ++size;
        if (isEmpty()) {
            first = last;
        }
        else {
            oldLast.next = last;
            last.pre = oldLast;
        }
    }

    // Dequeue and return a random item
    public Item dequeue() {
        if (isEmpty()) {
            throw new NoSuchElementException();
        }
        Node current = getNode(StdRandom.uniform(size));
        Item item = current.item;
        if (current.pre != null && current.next != null) {
            current.pre.next = current.next;
            current.next.pre = current.pre;
        }
        else if (current == first && current.next != null) {
            first = current.next;
            first.pre = null;
        }
        else if (current == last && current.pre != null) {
            last = current.pre;
            last.next = null;
        }
        else {
            first = last = null;
        }
        --size;
        return item;
    }

    // return a random item (but do not Dequeue it)
    public Item sample() {
        if (isEmpty()) {
            throw new NoSuchElementException();
        }
        Node current = getNode(StdRandom.uniform(size) );
        return current.item;
    }

    // return an independent iterator over items in random order
    public Iterator<Item> iterator() { return new ListIterator(); }
    private class ListIterator implements Iterator<Item> {
        private boolean[] isDisplayed = new boolean[size];
        private int num = size;
        private int index = StdRandom.uniform(size);

        private Node current = getNode(index);

        public boolean hasNext() {
            return num > 0;
        }

        public Item next() {
            if (num <= 0) { throw new NoSuchElementException(); }
            Item item = current.item;
            isDisplayed[index] = true;
            --num;
            // System.out.println("index: " + index);
            if (num > 0) {
                do {
                    index = StdRandom.uniform(size);
                } while(isDisplayed[index]);
                current = getNode(index);
            }
            return item;
        }

        public void remove() {
            throw new UnsupportedOperationException();
        }
    }

    private Node getNode (int x) {
        Node current = first;
        for(int i = 0; i < x; ++i) {
            current = current.next;
        }
        return current;
    }


    // unit testing (required)
    public static void main(String[] args) {
        RandomizedQueue<String> stringList1 = new RandomizedQueue<String>();
        stringList1.enqueue("1");
        stringList1.enqueue("2");
        stringList1.enqueue("3");
        System.out.println("The size of list is: " + stringList1.size());
        System.out.println("Sample: " + stringList1.sample());
        System.out.println("Sample: " + stringList1.sample());
        System.out.println("Sample: " + stringList1.sample());
        for(String s : stringList1) {
            System.out.println("Iteration: " + s);
        }
        System.out.println("Dequeue: " + stringList1.dequeue());
        System.out.println("Dequeue: " + stringList1.dequeue());
        System.out.println("Sample: " + stringList1.sample());
        System.out.println("Is the list empty? " + stringList1.isEmpty());
        System.out.println("Dequeue: " + stringList1.dequeue());
        System.out.println("Is the list empty? " + stringList1.isEmpty());
    }

}
```
