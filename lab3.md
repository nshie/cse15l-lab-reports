# Lab Report 3

## Part 1
A failure inducing input to the ArrayExamples program was to run the reverseInPlace method on an array such as {0, 1, 2}.

```
@Test 
public void testReverseInPlace() {
    int[] input1 = { 0, 1, 2 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{ 2, 1, 0 }, input1);
}
```

An input that would not induce failure could be just a singleton array, such as {0}.
```
@Test 
public void testReverseInPlace() {
    int[] input1 = { 0 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{ 0 }, input1);
}
```

The symptom was this error:
![Screenshot 1](/lab3-screenshot-testReverseInPlace-error.png)

The bug was that the old code was accessing the same array it was modifying (using `arr[i] = arr[arr.length - i - 1];`). This meant flipping the first half worked fine, but then the second half was just copying the already copied values, meaning the resulting array would be a mirror of the second half.
```
static void reverseInPlace(int[] arr) {
    for (int i = 0; i < arr.length; i += 1) {
        arr[i] = arr[arr.length - i - 1];
    }
}
```

The fix was to create a temporary new array that we could copy all the values to without modifying the original array, then pointing the original array reference to this new array.

```
static void reverseInPlace(int[] arr) {
    int[] newArray = new int[arr.length];
    for (int i = 0; i < arr.length; i += 1) {
        newArray[i] = arr[arr.length - i - 1];
    }
    arr = newArray;
}
```

This fixes the issue of copying values we just copied because we separated the array that we were reading from and the array we were writing to.

## Part 2

The grep command follows the pattern `grep [option...] [patterns] [file...]`

#### Option 1: -e
`grep -e [pattern] [file]` outputs every line which matches the pattern in the file.
This could be super useful if you wanted to find whether or not a certain word is used in a text, or want to find every time an author mentions a character for example.

```
njshi@nathan-laptop MINGW64 ~/Documents/School/CSE 15L/docsearch/technical (main)
$ grep -e explosion 911report/chapter-1.txt
    By 9:08, the mission crew commander at NEADS learned of the second explosion at the World Trade Center and decided against holding the fighters in military airspace away from Manhattan:
```
This shows that there is 1 line where the word 'explosion' is mentioned in chapter-1.txt

```
$ grep -e "United States" government/Post_Rate_Comm/Cohenetal_comparison.txt
and the United States
the Italian and United States systems are good candidates. The
to the Household Diary Study (United States Postal Service 2000),
activities reduce United States costs by about 25 percent (Cohen et        
In the United States, this is frequently called "firm holdout"
Many of the 18 million boxholders in the United States do not
the USO for the United States. That estimate assumed that the
Skimming in the United States Residential Delivery Market." In
Money Is: A Study of Rural Mail Delivery in the United States." In
United States Postal Service. 2000. "The Household Diary Study,
```
This shows every line containing the string 'United States' in Cohenetal_comparison.txt

#### Option 2: -c
`grep -c [pattern] [file]` suppresses the normal output (every line which matches the pattern) and instead outputs the number of matches in the file.
This option is particularly useful if you just want to see how many times a phrase is mentioned, for example if you were counting the number of expletives in a text.

```
njshi@nathan-laptop MINGW64 ~/Documents/School/CSE 15L/docsearch/technical (main)
$ grep -c cell biomed/1471-213X-1-15.txt
65
```
This shows there are 65 mentions of 'cell' in 1471-213X-1-15.txt

```
njshi@nathan-laptop MINGW64 ~/Documents/School/CSE 15L/docsearch/technical (main)
$ grep -c "Al Qaeda" 911report/chapter-1.txt 911report/chapter-13.4.txt
911report/chapter-1.txt:0
911report/chapter-13.4.txt:14
```
The output shows there were 0 mentions of Al Qaeda in chapter-1.txt, but 14 mentions of Al Qaeda in chapter-13.4

#### Option 3: -n
`grep -n [pattern] [file]` prefixes each occurence of the match with the line number from which it was found in the file.
This is useful if you not only want to know what the contents of the matched line were, but also where in the file that match occurred. A good example of this would be trying to find all the uses of a variable in your code, and where they happen.

```
njshi@nathan-laptop MINGW64 ~/Documents/School/CSE 15L/docsearch/technical (main)
$ grep -n "United States" government/Post_Rate_Comm/Cohenetal_comparison.txt
9:and the United States
79:the Italian and United States systems are good candidates. The
101:to the Household Diary Study (United States Postal Service 2000),
373:activities reduce United States costs by about 25 percent (Cohen et        
435:In the United States, this is frequently called "firm holdout"
490:Many of the 18 million boxholders in the United States do not
694:the USO for the United States. That estimate assumed that the
805:Skimming in the United States Residential Delivery Market." In
831:Money Is: A Study of Rural Mail Delivery in the United States." In
848:United States Postal Service. 2000. "The Household Diary Study,
```
This output shows us that the string 'United States' shows up in lines 9, 79, 101, 373, 435, 490, 694, 805, 831, 848 in Cohenetal_comparison.txt

```
njshi@nathan-laptop MINGW64 ~/Documents/School/CSE 15L/docsearch/technical (main)
$ grep -n draft government/Alcohol_Problems/DraftRecom-PDF.txt
11:conference, he and Daniel Pollock drafted recommendations for the
```
This shows there is only 1 instance of 'draft' in the file, which is on line 11

#### Option 4: -l
`grep -l [pattern] [file]` outputs only the file name if the file has a match for the pattern. This is extremely useful if you just want to see which files in the directory contain a match, and not what the actual lines are.

```
njshi@nathan-laptop MINGW64 ~/Documents/School/CSE 15L/docsearch/technical (main)
$ grep -l planet biomed/*.txt
biomed/1471-2180-2-22.txt
biomed/1471-2180-3-4.txt
```
This output shows that only two files, 1471-2180-2-22.txt and 1471-2180-3-4.txt, have any mention of 'planet'

```
njshi@nathan-laptop MINGW64 ~/Documents/School/CSE 15L/docsearch/technical (main)
$ grep -l "George Washington" government/*/*.txt
government/Gen_Account_Office/ai00134.txt
government/Gen_Account_Office/pe1019.txt
```
This shows only 2 files mention George Washington in the government folder, ai00134.txt and pe1019.txt

All command line options found from the [GNU Manual](https://www.gnu.org/software/grep/manual/grep.html)
