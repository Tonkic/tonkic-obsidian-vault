```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.PrintWriter;
import java.io.StreamTokenizer;

// 注意类名必须为 Main, 不要有任何 package xxx 信息

public class Main {

    public static void main(String[] args) throws IOException {

        // 把文件里的内容，load进来，保存在内存里，很高效，很经济，托管的很好

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        // 一个一个读数字

        StreamTokenizer in = new StreamTokenizer(br);

        // 提交答案的时候用的，也是一个内存托管区

        PrintWriter out = new PrintWriter(new OutputStreamWriter(System.out));

        while (in.nextToken() !=

                StreamTokenizer.TT_EOF) { // 文件没有结束就继续

            // n，二维数组的行

            int n = (int) in.nval;

            in.nextToken();

            // m，二维数组的列

            int m = (int) in.nval;

            // 装数字的矩阵，临时动态生成

            int[][] mat = new int[n][m];

            for (int i = 0; i < n; i++) {

                for (int j = 0; j < m; j++) {

                    in.nextToken();

                    mat[i][j] = (int) in.nval;

                }

            }

            out.println(maxSumSubmatrix(mat, n, m));

        }

        out.flush();

        br.close();

        out.close();

    }

}
```