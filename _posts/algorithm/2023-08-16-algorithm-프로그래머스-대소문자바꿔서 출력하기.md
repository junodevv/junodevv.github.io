---
title: "[Algorithm]대소문자 바꿔서 출력하기"
category: Algorithm
tags: algorithm programmers
---
프로그래머스 / [문제링크 - 대소문자 바꿔서 출력하기](https://school.programmers.co.kr/learn/courses/30/lessons/181949)

----
1. 문제 설명
2. 나의 풀이
3. 다른 풀이


---
# 문제 설명

영어 알파벳으로 이루어진 문자열 <code>str</code>이 주어집니다. 

각 알파벳을 대문자는 소문자로 소문자는 대문자로 변환해서 출력하는 코드를 작성해 보세요.

<hr>

<h5>제한사항</h5>

<ul>
<li>1 ≤ <code>str</code>의 길이 ≤ 20

<ul>
<li><code>str</code>은 알파벳으로 이루어진 문자열입니다.</li>
</ul></li>
</ul>

<hr>

<h5>입출력 예</h5>

<p>입력 #1</p>
```
aBcDeFg
```

<p>출력 #1</p>
```
AbCdEfG
```

<hr>

# 나의 풀이

```java
import java.util.Scanner;

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String str = sc.next();
        
        if (1<= str.length() && str.length()<=20){
            for(int i = 0; i < str.length(); i++){
                if(65 <= (int)str.charAt(i) && (int)str.charAt(i) <= 90){
                    System.out.print(Character.toLowerCase(str.charAt(i)));
                } else if(97 <= (int)str.charAt(i) && (int)str.charAt(i) <= 122){
                    System.out.print(Character.toUpperCase(str.charAt(i)));
                } else{
                    System.out.println("알파벳만 입력하세요");
                }
            }
        }
    }
}
```
### 코드 설명
- 아스키 코드(A= 65, Z= 90 / a= 97, z= 122)인 점을 활용해서 입력된 값이 알파벳인지, 대(소)문자인지 체크
- for 문과 charAt() 함수로 str의 각 자리의 문자를 체크하고 대/소 를 변환 해줌

### 헤맷던점
- char와 string 자료형에 따라 사용할수 있는 대소문자 변환 함수가 다른데 이걸 몰라서 헤맴
- [참고 - 자바 대소문자 변환 함수 ](/java/2023/08/16/java-대소문자변환.html)

### 성능요약
- 메모리: 65.3 MB 
- 시간: 252.70 ms

----

# 다른 풀이

```java
import java.util.*;

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String a = sc.next();
        String answer = "";

        //Stack <Character> stack = new Stack <> ();

        for(Character c : a.toCharArray()){
            if(Character.isUpperCase(c)){
                //stack.push(Character.toLowerCase(c));
                answer += Character.toLowerCase(c);
            }
            else if(Character.isLowerCase(c)){
                //stack.push(Character.toUpperCase(c));
                answer += Character.toUpperCase(c);
            }
        } 
        System.out.println(answer);
    }
}
```
### 코드 설명
- 입력받은 string을 toCharArray() 함수로 char의 배열로 바꾸고 그 배열을 이용해 for-each문을 만듦
- isUpperCase() 함수를 이용해 대/소문자를 구분함
- answer변수를 새로 만들어서 그 안에 char를 하나씩 추가함

### 성능 요약
- 메모리: 68.5 MB
- 시간: 197.5 ms

-----

```java
import java.util.Scanner;

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String a = sc.next();
        for(int i=0; i<a.length(); i++) {
            char c = a.charAt(i);
            if(Character.isUpperCase(c)) {
                System.out.print((char)(c+32));
            }
            else {
                System.out.print((char)(c-32));
            }
        }
    }
}
```
### 코드설명
- 대문자와 소문자의 아스키코드의 차이가 32임을 활용한 코드

### 성능 요약
- 메모리: 64 MB
- 시간: 165 ms

-----

# 끝

<h2 class="text-gray">reference</h2>

[프로그래머스 다른사람풀이](https://school.programmers.co.kr/learn/courses/30/lessons/181949/solution_groups?language=java)