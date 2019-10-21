# 25、leetcode937. 重新排列日志文件
解法一：
--  
思路：
--
    排序规则：字符日志在数字日志之前，字符日志先按照内容的字典序排列，如果内容相同再按照标识符排序，数字日志按照输入的顺序排序。所以使用两个集合来保存字符日志和数字日志，然后分别排序字符日志和数字日志，最后将数字日志添加到字符日志之后即可。  
代码： 
--
<pre>
/**
 * @author lihe
 * @date 2019/10/19 19:51
 * @descriptor
 */
public class ReorderLogFiles_937 {
    public static String[] reorderLogFiles(String[] logs) {
        List<String> letterList = new ArrayList<String>();
        List<String> numberList = new ArrayList<String>();
        for (int i = 0; i < logs.length; i++) {
            String s = logs[i];
            String left = s.substring(0,s.indexOf(" "));//左
            String right = s.substring(s.indexOf(" ")+1,s.length());//右
            if(right.charAt(0) >= '0' && right.charAt(0) <= '9'){
                numberList.add(s);
            }else{
                letterList.add(s);
            }
        }
        letterList.sort(new Comparator<String>() {
            @Override
            public int compare(String s1, String s2) {
                String s1Left = s1.substring(0,s1.indexOf(" "));
                String s1Right = s1.substring(s1.indexOf(" ")+1,s1.length());
                String s2Left = s2.substring(0,s2.indexOf(" "));
                String s2Right = s2.substring(s2.indexOf(" ")+1,s2.length());
                if(!s1Right.equals(s2Right))
                    return s1Right.compareTo(s2Right);
                return s1Left.compareTo(s2Left);
            }
        });
        letterList.addAll(numberList);
        return letterList.toArray(new String[letterList.size()]);
    }
    public static void main(String[] args) {
        String[] strs = {"a19 2 3 1","g1 act car","zo4 4 7","ab1 off key dog","a8 act zoo"};
        String[] strings = reorderLogFiles(strs);
        System.out.println(Arrays.toString(strings));
    }
}
</pre>
解法二：
--  
思路：
--
    排序规则：字lambda表达式。    
代码： 
--
<pre>
class Solution {
    public String[] reorderLogFiles(String[] logs) {
        Arrays.sort(logs, (log1, log2) -> {
            String[] split1 = log1.split(" ", 2);
            String[] split2 = log2.split(" ", 2);
            boolean isDigit1 = Character.isDigit(split1[1].charAt(0));
            boolean isDigit2 = Character.isDigit(split2[1].charAt(0));
            if (!isDigit1 && !isDigit2) {
                int cmp = split1[1].compareTo(split2[1]);
                if (cmp != 0) return cmp;
                return split1[0].compareTo(split2[0]);
            }
            return isDigit1 ? (isDigit2 ? 0 : 1) : -1;
        });
        return logs;
    }
}
</pre>
