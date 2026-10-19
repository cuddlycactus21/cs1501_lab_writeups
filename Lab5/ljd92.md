# Lab 5: LZW
Luke Douglas
## Code Solution
```java
/*************************************************************************
 *  Compilation:  javac LZW.java
 *  Execution:    java LZW - < input.txt   (compress)
 *  Execution:    java LZW + < input.txt   (expand)
 *  Dependencies: BinaryIn.java BinaryOut.java
 *
 *  Compress or expand binary input from standard input using LZW.
 *
 *  WARNING: STARTING WITH ORACLE JAVA 6, UPDATE 7 the SUBSTRING
 *  METHOD TAKES TIME AND SPACE LINEAR IN THE SIZE OF THE EXTRACTED
 *  SUBSTRING (INSTEAD OF CONSTANT SPACE AND TIME AS IN EARLIER
 *  IMPLEMENTATIONS).
 *
 *  See <a href = "http://java-performance.info/changes-to-string-java-1-7-0_06/">this article</a>
 *  for more details.
 *
 *************************************************************************/
package cs1501_lzw_lab;

public class LZW {
    private static final int R = 256; // number of input chars
    private static final int L = 65536; // number of codewords = 2^maxW
    private static final int maxW = 16; // maximum width

    public static void compress() {
        String input = BinaryStdIn.readString();
        TST<Integer> st = new TST<Integer>();
        for (int i = 0; i < R; i++)
            st.put("" + (char) i, i);
        int code = R + 1; // R is codeword for EOF
        int variableW = 9; // current codeword width

        while (input.length() > 0) {
            String s = st.longestPrefixOf(input); // Find max prefix match s.
            BinaryStdOut.write(st.get(s), variableW); // Print s's encoding.
            int t = s.length();
            if (code < L) {// Add s to symbol table.
                if (variableW < maxW && isAPowerOfTwo(code)) {
                    variableW++;
                }
                if (t < input.length())
                    st.put(input.substring(0, t + 1), code++);
            }
            input = input.substring(t); // Scan past s in input.
        }
        BinaryStdOut.write(R, variableW);
        BinaryStdOut.close();
    }

    public static void expand() {
        String[] st = new String[L];
        int i; // next available codeword value
        int variableW = 9;

        // initialize symbol table with all 1-character strings
        for (i = 0; i < R; i++)
            st[i] = "" + (char) i;
        st[i++] = ""; // (unused) lookahead for EOF

        int codeword = BinaryStdIn.readInt(variableW);
        if (codeword == R)
            return; // expanded message is empty string
        String val = st[codeword];

        while (true) {
            BinaryStdOut.write(val);
            codeword = BinaryStdIn.readInt(variableW);
            if (codeword == R)
                break;
            String s = st[codeword];
            if (i == codeword)
                s = val + val.charAt(0); // special case hack
            if (i < L) {
                st[i++] = val + s.charAt(0);
                if (variableW < maxW && isAPowerOfTwo(i)) {
                    variableW++;
                }
            }
            val = s;
        }
        BinaryStdOut.close();
    }

    private static boolean isAPowerOfTwo(int x) {
        // if x is a power of two, then it's of the form 0...010...0.
        // when we subtract one, we get 0...001...1.
        // anding them yields zero exactly.
        return (x & (x - 1)) == 0;
    }
}
```
## Code Explanation
Essentially, all I changed from the original LZW implementation was adding `variableW` variables in both the `compress()` and `expand()` methods to keep track of the current codeword width. Whenever the next codeword we read or write needs to increase its width, we increment `variableW`. The tricky part was figuring out when to do this, but basically, whenever the next codeword you write into the codebook will be a power of two, the width needs to increase.
## Time and Space Analysis
`compress()`'s `.substring()` calls are each $O(n)$, and we call it at least once at each iteration of the for loop, so the time complexity is $O(n^2)$. `expand()` is $O(n)$ for time because in the case where each character in the message appears exactly once, there are just as many codewords as characters and we run our while loop $n$ times.
Because `compress()` uses a TST, its space complexity is $O(n)$, since it creates a new node for every codeword added to it. `expand()`'s space complexity is $O(n)$ because it has to write out all `n` characters to the output file.