```java
import java.util.ArrayList;
import java.util.Arrays;

import edu.princeton.cs.algs4.In;
import edu.princeton.cs.algs4.StdDraw;
import edu.princeton.cs.algs4.StdOut;

// finds all line segments containing 4 or more points
public class FastCollinearPoints {
    private Point[] pointArray;
    private Point[] arrayCopy;
    private ArrayList<LineSegment> lineSegmentList = new ArrayList<LineSegment>();
    public FastCollinearPoints(Point[] points) {
        if (points == null) {
            throw new IllegalArgumentException();
        }
        pointArray = points.clone();
        for (Point p : points) {
            if (p == null)
                throw new IllegalArgumentException();
        }
        Arrays.sort(pointArray);
        for (int i = 0; i < pointArray.length - 1; ++i) {
            if (pointArray[i].compareTo(pointArray[i + 1]) == 0) {
                throw new IllegalArgumentException();
            }
        }

        int i = 0, j = 0, k = 0;
        Point currentPoint = new Point(0, 0);
        arrayCopy = pointArray.clone();
        for (i = 0; i < arrayCopy.length; ++i) {
            arrayCopy = pointArray.clone();
            currentPoint = arrayCopy[i];
            Arrays.sort(arrayCopy, currentPoint.slopeOrder());
            for (j = 0; j < arrayCopy.length; j += k) {
                k = 1;
                while ((j + k) < arrayCopy.length && currentPoint.slopeTo(arrayCopy[j + k]) == currentPoint.slopeTo(arrayCopy[j])) {
                    ++k;
                }
                if (k >= 3 && currentPoint.compareTo(arrayCopy[j]) < 0) {
                    lineSegmentList.add(new LineSegment(currentPoint, arrayCopy[j + k - 1]));
                }
            }
        }
    }   
    // the number of line segments    
    public int numberOfSegments() {
        return lineSegmentList.size();
    }
    // the line segments        
    public LineSegment[] segments() {
        return lineSegmentList.toArray(new LineSegment[0]);
    }
    public static void main(String[] args) {
        // read the n points from a file
        In in = new In(args[0]);
        int n = in.readInt();
        Point[] points = new Point[n];
        for (int i = 0; i < n; i++) {
            int x = in.readInt();
            int y = in.readInt();
            points[i] = new Point(x, y);
        }
    
        // draw the points
        StdDraw.enableDoubleBuffering();
        StdDraw.setXscale(0, 32768);
        StdDraw.setYscale(0, 32768);
        for (Point p : points) {
            p.draw();
        }
        StdDraw.show();
    
        // print and draw the line segments
        FastCollinearPoints collinear = new FastCollinearPoints(points);
        for (LineSegment segment : collinear.segments()) {
            StdOut.println(segment);
            segment.draw();
        }
        StdDraw.show();
    }                      
 }
```
