# Threads Lab 1

## Instructions

The starter code is supposed to find the sum of two inclusive integer ranges in
parallel but it’s not getting the correct result. Fix the code so that it gives
the correct `sum`.

## Example

```java
// Input
2 6
1 4

// Output
30
```

## Starter Code

```java
import java.util.Scanner;

public class Main {
    private static final long mainThreadId = Thread.currentThread().getId();

    public static void main(String[] args) throws InterruptedException {
        Scanner scanner = new Scanner(System.in);

        int start1 = scanner.nextInt();
        int end1 = scanner.nextInt();

        int start2 = scanner.nextInt();
        int end2 = scanner.nextInt();

        RangeAdder adder1 = new RangeAdder(start1, end1);
        RangeAdder adder2 = new RangeAdder(start2, end2);

        long partialSum1 = adder1.getSum();
        long partialSum2 = adder2.getSum();

        long sum = partialSum1 + partialSum2; // The sum is always 0. Fix it.

        System.out.println(sum);
    }

    // DO NOT modify the RangeAdder class
    static class RangeAdder extends Thread {

        int start;
        int end;

        private volatile long sum = 0;

        public RangeAdder(int start, int end) {
            this.start = start;
            this.end = end;
        }

        @Override
        public void run() {
            final long currentId = Thread.currentThread().getId();

            if (currentId == mainThreadId) {
                throw new RuntimeException("You must start a new thread!");
            }

            long total = 0;
            for (int i = start; i <= end; i++) {
                total += i;
            }

            this.sum = total;
        }

        public long getSum() {
            return sum;
        }
    }
}
```
