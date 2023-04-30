Download Link: https://assignmentchef.com/product/solved-cs-2210a-data-structures-and-algorithms-assignment-2-file-compression-with-hash-tables
<br>
<h1>1           Overview</h1>

For this assignment you are required to write a Java program that reads a file and compresses it using the algorithm described below. The <em>compression ratio </em>(defined as the size of the original file divided by the size of the compressed file) achieved by your program will depend on the input file.

You will be provided with an un-compression program, which receives as input a compressed file produced by your program and gives as output the original uncompressed file.

<h1>2           The Compression Algorithm</h1>

The input file could be a text file, an image file, an audio file, or any other kind of binary file, so we will consider that the input file is just a sequence of bytes. Each byte can be interpreted in Java as a character. For your reference, a table with all 8-bit characters and their ASCII codes is given in the course’s website. An observation that you might find useful is that any integer between 0 and 255 can be stored in one byte and it can be converted into a character through casting. So, for example, (char) 0 is the NULL character, (char) 1 is the SOH character, and (char) 97 is ’<em>a</em>’. Similarly, an 8-bit character can be converted to an integer through casting; for example (int)’a’ is 97.

The compression algorithm will map strings of characters into integer codes; the compressed file will store these codes. The algorithm uses a dictionary, that you have to implement using a hash table. The dictionary will store records of the form (string, code), which are used during the compression process; the string field in each record is used as the key for that record. At the beginning, the algorithm will store in the dictionary the following 256 (string,code) pairs: (<em>s</em><sub>0</sub><em>,</em>0)<em>,</em>(<em>s</em><sub>1</sub><em>,</em>1)<em>,… </em>(<em>s</em><sub>255</sub><em>,</em>255), where each string <em>s<sub>i</sub></em>, <em>i </em>= 0<em>,…,</em>255, has length one and it contains the single character (char)<em>i</em>. Therefore, string <em>s</em><sub>0 </sub>contains the NULL character (as (char)0 = NULL), <em>s</em><sub>1 </sub>contains the SOH character (as (char)1 = SOH), <em>s</em><sub>97 </sub>is the string “<em>a</em>” (as (char)97 = ’a’), and so on. The integer code associated with string <em>s<sub>i </sub></em>is the number <em>i</em>.

Once the dictionary has been initialized, the compression algorithm opens the input file and reads its contents one byte at a time. The compression algorithm is described below.

<ol>

 <li>Assign the value 256 to variable <em>n<sub>c </sub></em>(this is the smallest code that has not yet been assigned to any of the strings stored in the dictionary). Read the first byte from the input file and store it into variable <em>p<sub>b</sub></em>.</li>

 <li>Read one by one bytes from the input file and append them to <em>p<sub>b </sub></em>until the <strong>longest </strong>sequence of bytes <em>p<sub>b </sub></em>is read whose corresponding string <em>p </em>(obtained by first converting each byte in <em>p<sub>b </sub></em>to a character and then concatenating all these characters into a string), is already stored in the dictionary. Get the record (<em>p,code<sub>p</sub></em>) from the dictionary.</li>

 <li>Write <em>p</em>’s associated code, <em>code<sub>p</sub></em>, to the compressed file.</li>

 <li>If all the input file has been processed, then terminate. Otherwise, read the next byte <em>b </em>and convert it to a character <em>c</em>.</li>

 <li>Append character <em>c </em>to string <em>p </em>to form a new string <em>p</em><sup>′</sup>. Assign to string <em>p</em><sup>′ </sup>the integer code <em>n<sub>c</sub></em>, and then insert the pair (<em>p</em><sup>′</sup><em>,n<sub>c</sub></em>) in the dictionary. Clear the sequence <em>p<sub>b </sub></em>and then store <em>b </em>in <em>p<sub>b </sub></em>(so <em>p<sub>b </sub></em>contains a single byte). Increase the value of <em>n<sub>c </sub></em>and then go back to Step 2.</li>

</ol>

To clarify the way in which the algorithm works, let us try it on a text input file that contains the string “<em>aaabbbb</em>”. Remember that the dictionary initially contains 256 pairs (string,code) for the 256 different bytes that might appear in the input file.

The algorithm reads the first byte from the input file and converts it to a character: ’<em>a</em>’. Note that the longest string from the input that is already stored in the dictionary is “<em>a</em>”. Hence, in Step 2 of the algorithm the pair (“<em>a</em>”,97) is extracted from the dictionary, and in Step 3 the code 97 is written in the output file:

<table width="93">

 <tbody>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(“<em>a</em>”,97)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(“<em>b</em>”,98)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

 </tbody>

</table>

<strong>Input File    Compressed File          Dictionary </strong><em>a aabbbb  </em>97

In the above figure there is a box surrounding the part of the input that has already been processed.

Since ’<em>a</em>’ is followed in the input file by another ’<em>a</em>’, then in Steps 4-5, the algorithm concatenates them to get the new string “<em>aa</em>”. This string is assigned the smallest available code: 256 and then, the pair (“<em>aa</em>”,256) is inserted in the dictionary:

<table width="93">

 <tbody>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(“<em>a</em>”,97)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(“<em>b</em>”,98)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(“<em>aa</em>”,256)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

 </tbody>

</table>

<strong>Input File    Compressed File          Dictionary </strong><em>a aabbbb  </em>97

After step 5 the value of <em>n<sub>c </sub></em>is increased to 257 and <em>p<sub>b </sub></em>contains only the second byte of the input file. Observe that so far the algorithm has only output code for the first character ’<em>a</em>’ of the input.

In the second iteration of the algorithm <em>p<sub>b </sub></em>is converted to string <em>p </em>=“<em>a</em>”; since <em>p </em>is already in the dictionary the next byte, corresponding to the third ’<em>a</em>’, is read and appended to the end of <em>p<sub>b </sub></em>to get the string <em>p </em>= “<em>aa</em>”. This string “<em>aa</em>” is the longest string that is already stored in the dictionary, so the code 256 associated to “<em>aa</em>” is written to the output file.

Since “<em>aa</em>” is followed by the character ’<em>b</em>’, in Step 5 of the algorithm the string “<em>aab</em>” is assigned code 257, and the pair (“<em>aab</em>”,257) is entered into the dictionary:

<table width="93">

 <tbody>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(“<em>a</em>”,97)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(“<em>b</em>”,98)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(“<em>aa</em>”,256)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(”<em>aab</em>”,257)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

 </tbody>

</table>

<strong>Input File Compressed File Dictionary </strong><em>aaa bbbb </em>97 256

At this point the algorithm has processed “<em>aaa</em>”, so the next character from the input that is read is ’<em>b</em>’; “<em>b</em>” is the longest string that is already stored in the dictionary, so its code, 98, is output. As “<em>b</em>” is followed by ’<em>b</em>’, the pair (“<em>bb</em>”,258) is entered in the dictionary in Step 5.

<strong>Input File        Compressed File        Dictionary</strong>

<table width="93">

 <tbody>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(”<em>a</em>”,97)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(”<em>b</em>”,98)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(”<em>aa</em>”,256)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(”<em>aab</em>”,257)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(”<em>bb</em>”,258)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

 </tbody>

</table>

<em>aaab bbb                 </em>97 256 98                                                ···

The algorithm has already processed “<em>aaab</em>” from the input. Hence, the longest string from the remaining input that is already in the dictionary is “<em>bb</em>”. The algorithm outputs its code, 258, and then the pair (“<em>bbb</em>”,259) is stored in the dictionary.

<table width="93">

 <tbody>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(”<em>a</em>”,97)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(”<em>b</em>”,98)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(”<em>aa</em>”,256)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(”<em>aab</em>”,257)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(”<em>bb</em>”,258)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(”<em>bbb</em>”,259)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

 </tbody>

</table>

<strong>Input File Compressed File Dictionary </strong><em>aaabbb b </em>97 256 98 258

The only character that has not been processed is the final ’<em>b</em>’; for this, code 98 is written to the compressed file and then he algorithm terminates.

<table width="93">

 <tbody>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(”<em>a</em>”,97)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(”<em>b</em>”,98)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(”<em>aa</em>”,256)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(”<em>aab</em>”,257)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(”<em>bb</em>”,258)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

  <tr>

   <td width="93">(”<em>bbb</em>”,259)</td>

  </tr>

  <tr>

   <td width="93">···</td>

  </tr>

 </tbody>

</table>

<strong>Input File    Compressed File          Dictionary </strong><em>aaabbbb    </em>97 256 98 258 98

Note that in the above example, for illustration purposes, the pairs (string,code) were shown in increasing order of code value in the dictionary, but this will not be the case in your program since you will implement the dictionary using a hash table.

<h2>2.1         Size of the Dictionary</h2>

For very long input files the number of pairs (string,code) that might be entered in the dictionary could be very large. In order to keep the size of the integer codes bounded, we impose a limit of 4096 on the maximum number of different records that can be stored in the dictionary. Hence, every code that the algorithm outputs to the compressed file will be 12 bits long (as 2<sup>12 </sup>= 4096).

<strong>Important note. </strong>After your program has inserted 4096 records in the dictionary no more records can be added (as there are no more available integer codes). This is important, as otherwise your compressed file will not be decompressed correctly. If the dictionary already contains 4096 codes, then in Step 5 of the algorithm no new record will be inserted in the dictionary.

<h2>2.2         Writing Integer Codes to the Compressed File</h2>

To simplify your work, we provide a Java class, MyOutput, that you can use to write 12-bit codes in the output file. This class contains method

MyOutput.output(int code, BufferedOutputStream out); that writes a 12 bit integer code to the output file out. Since it is only possible to write whole bytes to the output file, the above method might temporarily keep in a buffer 4 bits from some of the codes. These 4 bits are concatenated with the 12 bits of the next code to produce a 2-byte output. If this is confusing to you, you do not need to understand the explanation of how method MyOutput.output works, you just need to understand how to use it.

It is very important that before you close the output file you invoke method

MyOutput.flush(BufferedOutputStream out); to send to the output file any bits that are still left in the buffer.

<h1>3           Classes to Implement</h1>

You are to implement at least 4 Java classes: DictEntry, Dictionary, DictionaryException, and Compress. You can implement more classes if you need to. You <strong>must </strong>write all the code yourself. You cannot use code from the textbook, the Internet, or any other sources. You <strong>cannot </strong>use the standard Java Hashtable class.

<h2>3.1         DictEntry</h2>

This class represents an entry in the dictionary, associating a string key with an integer code. For this class, you must implement all and only the following public methods:

<ul>

 <li>public DictEntry(String key, int code): A constructor which returns a new DictEntry with the specified key and code.</li>

 <li>public String getKey(): Returns the key in the DictEntry.</li>

 <li>public int getCode(): Returns the code in the DictEntry.</li>

</ul>

You can implement any other methods that you want to in this class, but they must be declared as private methods (i.e. not accessible to other classes).

<h2>3.2         Dictionary</h2>

This class implements a dictionary. You must implement the dictionary using a hash table. You will decide on the size of the table, keeping in mind that at most 4096 items will be stored in it and the size of the table must be a prime number.

You must design your hash function so that it produces few collisions. (A bad hash function that induces many collisions will result in a lower mark.) Collisions must be resolved using separate chaining.

For this class, you must implement all the public methods in the following interface:

public interface DictionaryADT { public int insert (DictEntry pair) throws DictionaryException; public void remove (String key) throws DictionaryException; public DictEntry find (String key); public int numElements();

}

The description of these methods follows:

<ul>

 <li>public int insert(DictEntry pair) throws DictionaryException: Inserts the given pair (string,code) in the dictionary. This method must throw a DictionaryException (see below) if the key associated with pair, getKey(), is already in the dictionary.</li>

</ul>

You are required to implement the dictionary using a hash table with separate chaining. To determine how good your design is, we will count the number of collisions produced by your hash function. This method will return the value 1 if the insertion of object pair produces a collision, and it will return the value 0 otherwise. In other words, if for example your hash function is <em>h</em>(<em>k</em>) and the name of your table is <em>T</em>, this method will return the value 1 if <em>T</em>[<em>h</em>(pair.getKey())] already stores at least one element; it will return 0 if <em>T</em>[<em>h</em>(pair.getKey())] is empty.

<ul>

 <li>public void remove(String key) throws DictionaryException: Removes the entry with the given key from the dictionary. This method must throw a DictionaryException (see below) if the key is not in the dictionary.</li>

 <li>public DictEntry find(String key): Returns the DictEntry object stored in the dictionary with the given key, or null if no entry in the dictionary has the given key.</li>

 <li>public int numElements(): Returns the number of DictEntry objects stored in the dictionary.</li>

</ul>

Since your Dictionary class must implement all the methods of the DictionaryADT interface, the declaration of your method should be as follows:

public class Dictionary implements DictionaryADT

You can download the file DictionaryADT.java from the course’s website and place it in the same directory as your Dictionary.java class. This will allow the Java compiler to verify that your class implements all required methods.

The only other public method that you can implement in the Dictionary class is the constructor method, which must be declared as follows

public Dictionary(int size)

this returns an empty dictionary of the specified size.

You can implement any other methods that you want to in this class, but they must be declared as private methods (i.e. not accessible to other classes).

<h2>3.3         DictionaryException</h2>

This is just the class implementing the class of exceptions thrown by the insert and remove methods of Dictionary. See the class notes on exceptions.

<h2>3.4         Compress</h2>

This class contains the main method, declared with the usual method header:

public static void main(String[] args)

You can implement any other methods that you want to in this class, but they must be declared as private methods (i.e. not accessible to other classes).

The input to the program will consist of a file. The output will be the compressed file. To execute your program you must type java Compress file

where file is the name of the input file. The compressed file must be stored in a file with the same name as the original one, but with extension ”.zzz”. For example if the name of the input file is test then the name of the compressed file must be test.zzz.

<h1>4           Testing your Program</h1>

We will perform two kinds of tests on your program: (1) tests for your implementation of the dictionary, and (2) tests for your file compressor. For testing the dictionary we will run a test program called TestDict which checks whether your dictionary works as specified. We will supply you with a copy of TestDict so you can use it to test your implementation.

For testing your file compressor we will give your program some input files, check the sizes of the compressed files, and then we will decompress them and verify that the original files are restored. The Java program to decompress files is given in the course’s website. Compile the program and then run it by typing:

java Decompress file

where file is the name of the compressed file <strong>without </strong>the “.zzz” extension. The program will produce a file with extension “.unc”. You can use, for example, the cmp UNIX utility to compare the original file with the decompressed one. By typing

cmp file1 file2

the utility will report the first character where the two files differ, if any. If the files are identical, the utility terminates without producing any output.

<h1>5           Coding Style</h1>

Your mark will be based partly on your coding style.

<ul>

 <li>Variable and method names should be chosen to reflect their purpose in the program.</li>

 <li>Comments and indenting should be used to improve readability.</li>

 <li>No variable declarations should appear outside methods (“instance variables”) unless they contain data which is to be maintained in the object from call to call. In other words, variables which are needed only inside methods, whose value does not have to be remembered until the next method call, should be declared inside those methods.</li>

 <li>All variables declared outside methods (“instance variables”) should be declared private (not protected), to maximize information hiding. Any access to these variables should be done with accessor methods (like getKey() for DictEntry)</li>

</ul>

<h1>6           Hints</h1>

You can open a file for reading as follows:

BufferedInputStream in;

in = new BufferedInputStream(new FileInputStream(inputFile));

where inputFile is a String storing the name of the input file. The output file can be opened in a similar way:

BufferedOutputStream out;

out = new BufferedOutputStream(new FileOutputStream(outputFile));

To read one byte (character) from the input file you can use method read() from the BufferedInputStream class. The end of file is detected when the read() method returns the value −1. Note that this method returns an integer value, so you must cast it into a char.

<h1>7           Marking</h1>

Your mark will be computed as follows.

<ul>

 <li>Program compiles, produces meaningful output: 2 marks.</li>

 <li>Dictionary tests pass: 4 marks.</li>

 <li>Compress tests pass: 4 marks.</li>

 <li>Coding style: 2 marks.</li>

 <li>Hash table implementation: 4 marks.</li>

 <li>Compress program implementation: 4 marks.</li>

</ul>

<h1>8           Handing In Your Program</h1>

To submit your program, login to OWL and submit your java files from there.

Read the tutorials posted in the course’s website to learn how to copy your program to your Gaul directory and how to test it there. Remember that the TA’s will test your programs on Gaul. Please read also the tutorials on how to configure Eclipse to read command line arguments. Please <strong>do not </strong>put your code in sub-directories or bundle your classes into a package.

If you re-submit your program after October 21 please send me an email message to let me know. I will inform the TA’s so they take the latest program submitted as the final version, and will deduct marks accordingly if it is late.