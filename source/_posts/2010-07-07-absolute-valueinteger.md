---
layout: post
title: "get absolute value(integer)"
date: 2010-7-7
wordpress_id: 244
comments: true
tags: ["absolute", "bit", "c", "code", "complement", "math", "math", "priority"]
categories:
- code
---
<meta name="_edit_last" content="1" />
<meta name="_su_description" content="how to use bit operator and simple +-*/ to get absolute number." />
<meta name="_su_keywords" content="bit,operator,absolute,complement,number,priority" />
<meta name="_su_rich_snippet_type" content="none" />
<meta name="_su_title" content="bit operators absolute complement number priority" />
<meta name="views" content="1060" />
<meta name="_wp_old_slug" content="absolute-numberinteger" />
How to get absolute (integer) only use bit operators and +-*/.(In gcc version 4.4.3 (Ubuntu 4.4.3-4ubuntu5)&nbsp; )
<ul>
	<li>if number &gt;= 0 , then number &gt;&gt; 31 == 0</li>
</ul>
<ul>
	<li>if number &lt; 0&nbsp;&nbsp; , then number &gt;&gt; 31 == 0xFFFFFFFF</li>
</ul>
If some number exclusive or( xor ) 0xFFFFFFFF means each bit becomes its "opposite" number. (1 becomes 0, and 0 becomes 1).And If a number xor 0x00000000 means nothing happened to this number. So:
<ul>
	<li>number ^ 0xFFFFFFFF = ~number</li>
	<li>number ^ 0x00000000 = number</li>
</ul>
In this question, you should also know something about<a href="http://en.wikipedia.org/wiki/Signed_number_representations#Ones.27_complement"> complement number(see two's complement)</a>. When a number is negative, |number| = ~number +1 or you can use |number| = !number + 1. When a number is positive, |number| = number.

If you know above, then the goal of |x| = (x^(x&gt;&gt;31))+(~(x&gt;&gt;31)+1).

(x^(x&gt;&gt;31)) means:
<ul>
	<li>x &gt;= 0, x^(x&gt;&gt;31) = x^0x00000000 = x</li>
	<li>x &lt; 0, x^(x&gt;&gt;31) = x^0xFFFFFFFF = ~x</li>
</ul>
And (~(x&gt;&gt;31)+1)means:
<ul>
	<li>x &gt;= 0, ~(x&gt;&gt;31)+1 = 0xFFFFFFFF+1 = 0</li>
	<li>x &lt; 0, ~(x&gt;&gt;31)+1 = 0x00000000+1 = 1</li>
</ul>
And we should take care of those brackets, because the priorities of operators are: ~ <span style="color: rgb(128, 0, 0); font-size: 16pt;"><strong>&gt;</strong></span> */ <span style="color: rgb(153, 51, 0); font-size: 16pt;"><strong>&gt;</strong></span> + - <strong><span style="color: rgb(153, 51, 0); font-size: 16pt;">&gt;</span></strong> bit operators(&amp;,^,|)
<table>
<tbody>
<tr>
<th>Level</th>
<th>Operator</th>
<th>Description</th>
<th>Grouping</th>
</tr>
<tr>
<td>1</td>
<td><tt>::</tt></td>
<td>scope</td>
<td>Left-to-right</td>
</tr>
<tr>
<td>2</td>
<td><tt>() [] . -&gt; ++ -- dynamic_cast static_cast  reinterpret_cast const_cast typeid</tt></td>
<td>postfix</td>
<td>Left-to-right</td>
</tr>
<tr>
<td rowspan="3">3</td>
<td><tt>++ -- ~ ! sizeof new delete</tt></td>
<td>unary  (prefix)</td>
<td rowspan="3">Right-to-left</td>
</tr>
<tr>
<td><tt>* &amp;</tt></td>
<td>indirection and reference (pointers)</td>
</tr>
<tr>
<td><tt>+ -</tt></td>
<td>unary sign operator</td>
</tr>
<tr>
<td>4</td>
<td><tt>(type)</tt></td>
<td>type casting</td>
<td>Right-to-left</td>
</tr>
<tr>
<td>5</td>
<td><tt>.* -&gt;*</tt></td>
<td>pointer-to-member</td>
<td>Left-to-right</td>
</tr>
<tr>
<td>6</td>
<td><tt>* / %</tt></td>
<td>multiplicative</td>
<td>Left-to-right</td>
</tr>
<tr>
<td>7</td>
<td><tt>+ -</tt></td>
<td>additive</td>
<td>Left-to-right</td>
</tr>
<tr>
<td>8</td>
<td><tt>&lt;&lt; &gt;&gt;</tt></td>
<td>shift</td>
<td>Left-to-right</td>
</tr>
<tr>
<td>9</td>
<td><tt>&lt; &gt; &lt;= &gt;=</tt></td>
<td>relational</td>
<td>Left-to-right</td>
</tr>
<tr>
<td>10</td>
<td><tt>== !=</tt></td>
<td>equality</td>
<td>Left-to-right</td>
</tr>
<tr>
<td>11</td>
<td><tt>&amp;</tt></td>
<td>bitwise AND</td>
<td>Left-to-right</td>
</tr>
<tr>
<td>12</td>
<td><tt>^</tt></td>
<td>bitwise XOR</td>
<td>Left-to-right</td>
</tr>
<tr>
<td>13</td>
<td><tt>|</tt></td>
<td>bitwise OR</td>
<td>Left-to-right</td>
</tr>
<tr>
<td>14</td>
<td><tt>&amp;&amp;</tt></td>
<td>logical AND</td>
<td>Left-to-right</td>
</tr>
<tr>
<td>15</td>
<td><tt>||</tt></td>
<td>logical OR</td>
<td>Left-to-right</td>
</tr>
<tr>
<td>16</td>
<td><tt>?:</tt></td>
<td>conditional</td>
<td>Right-to-left</td>
</tr>
<tr>
<td>17</td>
<td><tt>= *= /= %= += -= &gt;&gt;= &lt;&lt;= &amp;= ^= |=</tt></td>
<td>assignment</td>
<td>Right-to-left</td>
</tr>
<tr>
<td>18</td>
<td><tt>,</tt></td>
<td>comma</td>
<td>Left-to-right</td>
</tr>
</tbody>
</table>
