```java
import java.util.Iterator;
import java.util.NoSuchElementException;

public class Deque<Item> implements Iterable<Item> {

    private Node first, last;
    private int size;

    private class Node {
        Item item;
        Node next;
        Node pre;
    }
    // construct an empty deque
    public Deque() {

    }

    // is the deque empty?
    public boolean isEmpty() {
        return first == null || last == null;
    }

    // return the number of items on the deque
    public int size() {
        return size;
    }

    // add the item to the front
    public void addFirst(Item item) {
        if (item == null) {
            throw new IllegalArgumentException();
        }
        Node oldFirst = first;
        first = new Node();
        first.item = item;
        first.next = oldFirst;
        ++size;
        if (size == 1) {
            last = first;
        }
        else {
            oldFirst.pre = first;
        }
    }

    // add the item to the back
    public void addLast(Item item) {
        if (item == null) {
            throw new IllegalArgumentException();
        }
        Node oldLast = last;
        last = new Node();
        last.item = item;
        last.pre = oldLast;
        ++size;
        if (size == 1) {
            first = last;
        }
        else {
            oldLast.next = last;
        }
    }

    // remove and return the item from the front
    public Item removeFirst() {
        if (isEmpty()) {
            throw new NoSuchElementException();
        }
        Item item = first.item;
        first = first.next;
        if (isEmpty()) {
            last = first;
        }
        else {
            first.pre = null;
        }
        --size;
        return item;
    }

    // remove and return the item from the back
    public Item removeLast() {
        if (isEmpty()) {
            throw new NoSuchElementException();
        }
        Item item = last.item;
        last = last.pre;
        if (isEmpty()) {
            first = last;
        }
        else {
            last.next = null;
        }
        --size;
        return item;
    }

    // return an iterator over items in order from front to back
    public Iterator<Item> iterator() { return new ListIterator(); }
    private class ListIterator implements Iterator<Item> {
        private Node current = first;
        public boolean hasNext() { return current != null; }

        public Item next() {
            if (current == null) {
                throw new NoSuchElementException();
            }
            Item item = current.item;
            current = current.next;
            return item;
        }
        
        public void remove() {
            throw new UnsupportedOperationException();
        }
    }

    // unit testing (required)
    public static void main(String[] args) {
        Deque<String> stringList1 = new Deque<String>();
        String s1 = ""; 
        // addFirst addLast
        stringList1.addFirst("1");
        for (String s : stringList1) {
            System.out.println(s);
        }
        stringList1.addLast("2");
        for (String s : stringList1) {
            System.out.println(s);
        }
        // size
        System.out.println(stringList1.size());
        // removeFirst removeLast
        s1 = stringList1.removeFirst();
        System.out.println(s1);
        for (String s : stringList1) {
            System.out.println(s);
        }
        stringList1.addFirst("0");
        for (String s : stringList1) {
            System.out.println(s);
        }
        System.out.println("Is stringList1 empty? : " + stringList1.isEmpty());
        stringList1.removeLast();
        stringList1.removeFirst();
        System.out.println("Is stringList1 empty? : " + stringList1.isEmpty());
    }

}
```
