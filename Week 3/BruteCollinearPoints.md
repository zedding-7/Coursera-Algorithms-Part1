```java
import java.util.ArrayList;
import java.util.Arrays;
// finds all line segments containing 4 points
public class BruteCollinearPoints {
    private Point[] pointArray;
    private ArrayList<LineSegment> lineSegmentList = new ArrayList<LineSegment>();

    public BruteCollinearPoints(Point[] points) {
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

        Point currentPoint = new Point(0, 0);
        for (int i = 0; i < pointArray.length - 3; ++i) {
            for (int j = i + 1; j < pointArray.length - 2; ++j) {
                for (int k = j + 1; k < pointArray.length - 1; ++k) {
                    for (int l = k + 1; l < pointArray.length; ++l) {
                        currentPoint = pointArray[i];
                        if (currentPoint.slopeTo(pointArray[j]) == currentPoint.slopeTo(pointArray[k])
                        && currentPoint.slopeTo(pointArray[j]) == currentPoint.slopeTo(pointArray[l])) {
                            lineSegmentList.add(new LineSegment(currentPoint, pointArray[l]));
                        }
                    }
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
 }
```
