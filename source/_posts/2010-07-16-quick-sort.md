---
layout: post
title: "Quick Sort"
date: 2010-7-16
wordpress_id: 262
comments: true
tags: ["algorithm", "c", "cc", "optimiz", "quick", "see-hear", "sort"]
categories:
- code
---
<meta name="_edit_last" content="1" />
<meta name="_su_rich_snippet_type" content="none" />
<meta name="_su_description" content="Optimize QuickSort" />
<meta name="_su_keywords" content="Optimize,Quick,Sort" />
<meta name="_su_title" content="Quick Sort" />
<meta name="views" content="851" />
the simplest quick sort is like following:


{% codeblock %}#include&lt;iostream&gt;

using namespace std;

int Partition(int * a, int start, int end){
    swap(a[start],a[start+rand()%(end-start+1)]);
    int p = a[start];
    while(start&lt;end){
        while(start&lt;end &amp;&amp; a[end] &gt;= p) end--;
        swap(a[start],a[end]);
        while(start&lt;end &amp;&amp; a[start]&lt;= p) start++;
        swap(a[start],a[end];)
    }
    return start;
}

void QuickSort(int * a, int start, int end){

     if(end-start&gt;=1){
         int mid = Partition(a,start, end);
         QuickSort(a,start,mid-1);
         QuickSort(a,mid+1,end);
     }
}{% endcodeblock %}


But If you want do some optimization in QuickSort, first when the number of elements is small than 10, do insert sort not quick sort. Second, To find the near-like middle element of array, and not use the random one. Third, when the array is already sorted , do not use quick sort. and Reduce the function call. You will the following codes:


{% codeblock %}#include&lt;iostream&gt;

using namespace std;

void QuickSort(int *a, int start, int end){

    if(end - start &lt;= 0 ) // or you can throw some exceptions!
       return ;

// if the number of element is less than 8, do insert sort;

    if(end - start&lt;7){

      for(int i = start; i &lt;= end; i++){

         for(int j = i; j &gt; start &amp;&amp; a[j] &lt; a[j-1]; j--)

             swap(a[j],a[j-1]);

      }

      return;
    }

// check if the array is sorted.

    bool isSorted = true;
    for(int i = start; i &lt; end; i++){
       if(a[i]&gt;a[i+1])
         isSorted = false;
    }
    if(isSorted)
      return;

// get the near-like middle element, use 3 point middle method.

    int dm = start+(end-start)/2;

    int dl, dh;

    if(end-start&lt;40){

       int d = (end-start+1)/8;

       dl = mid(a, start,start+d,start+2*d);

       dm = mid(a,dm-d,dm,dm+d);

       dh = mid(a,end-2*d,end-d, end);

       dm = mid(a,dl,dm,dh);

    }

// do partition

    swap(a[start],a[dm]);

    int low=start, high = end, p = a[start];
    while(low&lt;high){
        while(low &lt; high &amp;&amp; a[high]&gt;= p) high--;
        swap(a[low],a[high]);
        while(low&lt;high &amp;&amp; a[low]&lt;= p) low ++;
        swap(a[low],a[high]);
    }
    QuickSort(a,start, low-1);
    QuickSort(a,low+1,end);
}{% endcodeblock %}
 
