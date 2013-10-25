---
author: admin
comments: true
date: 2007-07-16 02:28:23+00:00
layout: post
slug: devising-a-strategy-to-consistently-win-hangman-part-i
title: 'Devising a strategy to (consistently) win Hangman: Part I'
wordpress_id: 55
categories:
- .NET 2.0
- Off topic
---

Edit: Try my own version of [Hangman](http://apps.wadewegner.com/hangman/), written with Silverlight and WPF!




In an effort to distract myself from more productive endeavors, I started playing Hangman on [The Free Dictionary](http://www.thefreedictionary.com/) Web site today. They have an outstanding plugin that you can also add to your Google homepage (which is actually how I discovered it). The interface allows you to simply type the letter into the guess box (if you look below you'll see that my last guess was the letter "G"), and it updates it as you go:




![image](http://images.wadewegner.com/wordpress/content/binary/WindowsLiveWriter/Devisingastrategytoconsistentlywinningha_1117F/image_2.png)




During game play, the plugin constructs the man you are desperately trying to save. You can only make ten mistakes before the man is hanged:




![image](http://images.wadewegner.com/wordpress/content/binary/WindowsLiveWriter/Devisingastrategytoconsistentlywinningha_1117F/image_4.png)




So, as I was wasting my time today, I started wondering if there's a strategy to winning Hangman. Assuming that there is, I decided to try and figure it out.




I started with a few basic ideas:






  1. Certain letters are used more frequently than other letters.

  2. The frequency of letters are probably different given the total number of letters in the word.

  3. You can maximize your ability to win Hangman by playing the most common letter first, followed by the next most common letter, and so on.



I realize that there are probably more complicated theories (such as the most common letters changing based on discovered letters), but for this first part I decided that I wanted to keep it somewhat simple. I can complicate it more later on.




**_(If you aren't interested in _how_ I explored these ideas than jump to the end of this post to see my findings!)_**




My first order of business was to find a list of English words. After performing a few searches, I found what I was looking for: [Word-List.com](http://www.word-list.com/). They have word lists in many languages, and the lists are available as zipped text files separated by a newline. This made it very easy to parse.




I unzipped the file and added it to a new Visual Studio 2005 Console Application project. So that the file is always available in the root of the folder, I changed the **Copy to Output Directory** property to **Copy always**. Next, I wrote a method that loads all the words into a string array and saves each word to a particular file, based on the length of the word. For example, the words "dog" and "cat" are saved to the file "03.txt" whereas the words "father" and "mother" are saved to the file "06.txt". Here's the code:
    
    <span style="COLOR: rgb(0,0,255)">using</span> (<span style="COLOR: rgb(43,145,175)">StreamReader</span> input = <span style="COLOR: rgb(0,0,255)">new</span> <span style="COLOR: rgb(43,145,175)">StreamReader</span>(<span style="COLOR: rgb(163,21,21)">"words.english.txt"</span>))
    {
        <span style="COLOR: rgb(0,0,255)">string</span> contents = input.ReadToEnd().Trim();
        <span style="COLOR: rgb(0,0,255)">string</span>[] wordArray = contents.Split(<span style="COLOR: rgb(163,21,21)">'n'</span>);
    
        <span style="COLOR: rgb(0,0,255)">foreach</span> (<span style="COLOR: rgb(0,0,255)">string</span> word <span style="COLOR: rgb(0,0,255)">in</span> wordArray)
        {
            appendWord(word);
        }
    }

[](http://11011.net/software/vspaste)


Using the StreamReader, I loaded the file and split it into a string array based on the character 'n' (note the single, rather than double, quotes). This allows me to iterate through an array of words. I pass each word to the appendWord method, which calls the appendFile method. Passed in is a file name that's based on the length of the word, as well as the word itself.
    
    <span style="COLOR: rgb(0,0,255)">private</span> <span style="COLOR: rgb(0,0,255)">static</span> <span style="COLOR: rgb(0,0,255)">string</span> wordByLength = <span style="COLOR: rgb(163,21,21)">"{0}.txt"</span>;
    
    <span style="COLOR: rgb(0,0,255)">public</span> <span style="COLOR: rgb(0,0,255)">static</span> <span style="COLOR: rgb(0,0,255)">void</span> appendWord(<span style="COLOR: rgb(0,0,255)">string</span> word)
    {
        appendFile(<span style="COLOR: rgb(43,145,175)">String</span>.Format(wordByLength, word.Length.ToString().PadLeft(2, <span style="COLOR: rgb(163,21,21)">'0'</span>)), word);
    }




The appendFile method simply creates an instance of a StreamWriter and appends the word to the file (if it exists) or creates a new file (if it doesn't exist).
    
    <span style="COLOR: rgb(0,0,255)">public</span> <span style="COLOR: rgb(0,0,255)">static</span> <span style="COLOR: rgb(0,0,255)">void</span> appendFile(<span style="COLOR: rgb(0,0,255)">string</span> file, <span style="COLOR: rgb(0,0,255)">string</span> text)
    {
        <span style="COLOR: rgb(0,0,255)">using</span> (<span style="COLOR: rgb(43,145,175)">StreamWriter</span> writer = <span style="COLOR: rgb(43,145,175)">File</span>.AppendText(file))
        {
            writer.WriteLine(text);
            writer.Flush();
        }
    }

[](http://11011.net/software/vspaste)


Once I wrote all this and let it run, it took about 20-30 minutes to complete (I don't know exactly because I went to eat dinner). When I cam back, I had a directory of 23 files filed with sorted and ordered data:




![image](http://images.wadewegner.com/wordpress/content/binary/WindowsLiveWriter/Devisingastrategytoconsistentlywinningha_1117F/image_3.png)




Pretty cool, but not quite there!




Before I moved on, I wanted to know how many words had only two characters, three characters, and so on, all the way to 24. So, I wrote another method that loaded each of the aforementioned files, loaded them into an array, and determined the length of the array.
    
    <span style="COLOR: rgb(0,0,255)">foreach</span> (<span style="COLOR: rgb(0,0,255)">string</span> fileByLength <span style="COLOR: rgb(0,0,255)">in</span> <span style="COLOR: rgb(43,145,175)">Directory</span>.GetFiles(wordByLengthDirectory))
    {
        <span style="COLOR: rgb(0,0,255)">using</span> (<span style="COLOR: rgb(43,145,175)">StreamReader</span> input = <span style="COLOR: rgb(0,0,255)">new</span> <span style="COLOR: rgb(43,145,175)">StreamReader</span>(fileByLength))
        {
            <span style="COLOR: rgb(0,0,255)">string</span> contents = input.ReadToEnd().Trim().Replace(<span style="COLOR: rgb(163,21,21)">"rn"</span>, <span style="COLOR: rgb(163,21,21)">"n"</span>);
            <span style="COLOR: rgb(0,0,255)">string</span>[] wordArray = contents.Split(<span style="COLOR: rgb(163,21,21)">'n'</span>);
            <span style="COLOR: rgb(0,0,255)">int</span> wordLength = wordArray[0].Length;
    
            appendFile(wordCountByLength, wordLength.ToString().PadLeft(2, <span style="COLOR: rgb(163,21,21)">'0'</span>) + <span style="COLOR: rgb(163,21,21)">": "</span> + wordArray.Length);
        }
    }

[](http://11011.net/software/vspaste)


Nothing really complicated here. I loaded the files into a file array by using the Directory.GetFiles() method, replaced "rn" with "n" since the Environment.NewLine creates carriage return and a newline, split the array, and appended the length of the array (along with the number of characters in the word) into a file. Here were the results:




02: 61  
03: 627  
04: 2988  
05: 7198  
06: 14163  
07: 20452  
08: 27015  
09: 29824  
10: 29220  
11: 25021  
12: 19966  
13: 14683  
14: 9672  
15: 5890  
16: 3363  
17: 1808  
18: 838  
19: 428  
20: 197  
21: 81  
22: 40  
23: 17  
24: 5


Kind of interesting that there are more 9-letter words than any other words, eh? It also makes a really pretty looking graph:




![](http://images.wadewegner.com/wordpress/content/binary/graph1.gif)




While it's interesting, it doesn't really help me with my stated goal.




So, with the twenty-three independent files, I decided to find the most common letters, and sort them according to popularity. This, my friends, is the best part. Not only did I decide to use a generic list to contain the details of these files, but I also decided to use predicates and delegates to help me with my task! What fun!?!




First, I needed a collection to start the characters and counts in. For example, when I load the 61 words that contain two characters, I need a collection that keeps track of the frequency each character is used. To do this I defined a class called CharacterCounter, and then created a generic list. First, here's the class I created.
    
    <span style="COLOR: rgb(0,0,255)">public</span> <span style="COLOR: rgb(0,0,255)">class</span> <span style="COLOR: rgb(43,145,175)">CharacterCounter</span><span style="COLOR: rgb(43,145,175)">
    </span>{
        <span style="COLOR: rgb(0,0,255)">private</span> <span style="COLOR: rgb(0,0,255)">char</span> character;
        <span style="COLOR: rgb(0,0,255)">private</span> <span style="COLOR: rgb(0,0,255)">int</span> count;
    
        <span style="COLOR: rgb(0,0,255)">public</span> <span style="COLOR: rgb(0,0,255)">char</span> Character
        {
            <span style="COLOR: rgb(0,0,255)">get</span> { <span style="COLOR: rgb(0,0,255)">return</span> character; }
            <span style="COLOR: rgb(0,0,255)">set</span> { character = <span style="COLOR: rgb(0,0,255)">value</span>; }
        }
    
        <span style="COLOR: rgb(0,0,255)">public</span> <span style="COLOR: rgb(0,0,255)">int</span> Count
        {
            <span style="COLOR: rgb(0,0,255)">get</span> { <span style="COLOR: rgb(0,0,255)">return</span> count; }
            <span style="COLOR: rgb(0,0,255)">set</span> { count = <span style="COLOR: rgb(0,0,255)">value</span>; }
        }
    }

[](http://11011.net/software/vspaste)


Very simple class. It has two public properties: Character and Count. When used as a generic list, it allows me to create an object of characters with their associated counts (e.g. frequency used).




In determining the frequency each character is used based on the number of characters in the words, I first iterate through each of the files that contain the sorted words. This allows me to first find the ordered frequency of characters for words with two characters all the way to twenty-four characters. I then load the contents of each file into a string array, so that I can iterate through each of the words. Next, I create a character array out of the word, and iterate through each of the characters. It looks something like this:
    
    <span style="COLOR: rgb(0,0,255)">foreach</span> (<span style="COLOR: rgb(0,0,255)">string</span> fileByLength <span style="COLOR: rgb(0,0,255)">in</span> <span style="COLOR: rgb(43,145,175)">Directory</span>.GetFiles(wordByLengthDirectory))
    {
        <span style="COLOR: rgb(43,145,175)">List</span><CharacterCounter> characterCounters = <span style="COLOR: rgb(0,0,255)">new</span> <span style="COLOR: rgb(43,145,175)">List</span><CharacterCounter>();
        <span style="COLOR: rgb(0,0,255)">int</span> fileLength = 0;
    
        <span style="COLOR: rgb(0,0,255)">using</span> (<span style="COLOR: rgb(43,145,175)">StreamReader</span> input = <span style="COLOR: rgb(0,0,255)">new</span> <span style="COLOR: rgb(43,145,175)">StreamReader</span>(fileByLength))
        {
            <span style="COLOR: rgb(0,0,255)">string</span> contents = input.ReadToEnd().Trim().Replace(<span style="COLOR: rgb(163,21,21)">"rn"</span>, <span style="COLOR: rgb(163,21,21)">"n"</span>);
            <span style="COLOR: rgb(0,0,255)">string</span>[] wordArray = contents.Split(<span style="COLOR: rgb(163,21,21)">'n'</span>);
            <span style="COLOR: rgb(0,0,255)">if</span> (fileLength == 0)
            {
                fileLength = wordArray[0].Length;
            }
    
            <span style="COLOR: rgb(0,0,255)">foreach</span> (<span style="COLOR: rgb(0,0,255)">string</span> word <span style="COLOR: rgb(0,0,255)">in</span> wordArray)
            {
                <span style="COLOR: rgb(0,0,255)">char</span>[] characterArray = word.ToCharArray();
    
                <span style="COLOR: rgb(0,0,255)">foreach</span> (<span style="COLOR: rgb(0,0,255)">char</span> character <span style="COLOR: rgb(0,0,255)">in</span> characterArray)
                {
                    <span style="COLOR: rgb(0,128,0)">//...
    </span>            }
            }
        }
    }

[](http://11011.net/software/vspaste)


Once I am iterating through each of the characters, I need to start keeping track of the number of times the character appears. Notice that I created a generic list for my CharacterCounter class called characterCounters. Before I increment the character count, I first check to see if the character in question already exists within my list. This is performed using the following code:
    
    <span style="COLOR: rgb(43,145,175)">CharacterCounter</span> characterCounter = characterCounters.Find(<span style="COLOR: rgb(0,0,255)">delegate</span>(<span style="COLOR: rgb(43,145,175)">CharacterCounter</span> d)
    {
        <span style="COLOR: rgb(0,0,255)">return</span> d.Character == character;
    });




[](http://11011.net/software/vspaste)If the character exists, then the characterCounter list returns the CharacterCounter object that has a Character that equals the character I specified. For instance, if the specified character is "a", and it exists in the CharacterCounter list, then the characterCounter object is returned with an "a" for the Character and the number of times it has been found in the Count property.




If the character does not exist in the characterCounters list, then the characterCounter object is null. If it's null, I create a new instance of a CharacterCounter, set my values accordingly, and add it to the list. If it's not null, I increment the count:
    
    <span style="COLOR: rgb(0,0,255)">if</span> (characterCounter == <span style="COLOR: rgb(0,0,255)">null</span>)
    {
        characterCounter = <span style="COLOR: rgb(0,0,255)">new</span> <span style="COLOR: rgb(43,145,175)">CharacterCounter</span>();
        characterCounter.Character = character;
        characterCounter.Count = 1;
    
        characterCounters.Add(characterCounter);
    }
    <span style="COLOR: rgb(0,0,255)">else
    </span>{
        characterCounter.Count += 1;
    }

[](http://11011.net/software/vspaste)


This continues until I've gone through every word in the file. At this point, I have a generic CharacterCounter list filled with the number of times a character is found. Since I want to know the most commonly used characters, I need to sort my list so that the most common characters are first.




The following code will sort the characterCounters list by descending count:
    
    characterCounters.Sort(<span style="COLOR: rgb(0,0,255)">delegate</span>(<span style="COLOR: rgb(43,145,175)">CharacterCounter</span> cc0, <span style="COLOR: rgb(43,145,175)">CharacterCounter</span> cc1)<br></br>{<br></br>    <span style="COLOR: rgb(0,0,255)">return</span> cc1.Count.CompareTo(cc0.Count);<br></br>});




Using an anonymous delegate, you can tell the list to sort itself from the largest number to the smallest. Changing the code to the following will sort it from smallest to largest:
    
    characterCounters.Sort(<span style="COLOR: rgb(0,0,255)">delegate</span>(<span style="COLOR: rgb(43,145,175)">CharacterCounter</span> cc0, <span style="COLOR: rgb(43,145,175)">CharacterCounter</span> cc1)
    {
        <span style="COLOR: rgb(0,0,255)">return</span> cc0.Count.CompareTo(cc1.Count);
    });

[](http://11011.net/software/vspaste)


All this is done **_without_** having to use the IComparable interface! Amazing!




At this point, our list is sorted. All we need to do is create a string based on the ordered characters, and write it to a file:
    
    <span style="COLOR: rgb(0,0,255)">foreach</span> (<span style="COLOR: rgb(43,145,175)">CharacterCounter</span> characterCounter <span style="COLOR: rgb(0,0,255)">in</span> characterCounters)
    {
        orderedCharacters += characterCounter.Character.ToString();
    }
    appendFile(orderedFrequencyByLength, fileLength.ToString().PadLeft(2, <span style="COLOR: rgb(163,21,21)">'0'</span>) + <span style="COLOR: rgb(163,21,21)">": "</span> + orderedCharacters);




[](http://11011.net/software/vspaste)The code simply iterates through the characterCounters list, appends the value of the Character property to a string, and then appends the data to a string.




After all of this work, we can finally show the list.




# of characters in word: list of characters, from most frequent to least frequent




02: aoueyidbrtfslnmhgwzkvjcp  
03: aoeuirstdylmghpknbwfczvxj  
04: aeoirutslndpkymbhgcwfzvjxq  
05: aeriosnltuycdmhpbgkwfvzxjq  
06: earinoltsucdmphgbykfwvzxjq  
07: eairnoltsucdmpghbykfwvzxjq  
08: eaironlstucdmphgybfkwvzxjq  
09: eiarontslcudmphygbfkvwzxqj  
10: eiaorntslcupdmhygbfvkwzxqj  
11: eiaorntslcupmdhygbfvkwzxqj  
12: eiaontrslcupmhdygbfvzkxwqj  
13: eiaontrslcpumhdygbvfzxkqwj  
14: eioantsrlcpuhmdygbvfzxqkwj  
15: ieoantsrlcpuhmydgbvfzxqkwj  
16: eioatnrslcphumydgbvfzxqkwj  
17: ieoatnrsclphumydgbvfzxqkwj  
18: oeiatrnsclphmuydgbvfzxqkjw  
19: oiaetnrsclhpmyudgbvzfxqkwj  
20: oieatrnlchspymugdbvzfxjqk  
21: oietanlcrshpymgdubzfvjx  
22: oeticrahnslpymdugbxvzjq  
23: oiaetlhscnprmdyugbfx  
24: oihletapcyrdsnfgmzux


Going from the left to the right, we can tell the frequency of a letter in a word based on the number of characters in the word. For example, in a nine-letter word, the most frequently used characters are: e, i, a, r, o, n, etc.




Now, here's the real question: does knowing all of this help us to be a better Hangman player?




Well, what don't you try for yourself. Go to [The Free Dictionary](http://www.thefreedictionary.com/) and play a game of Hangman. Based on the number of characters in the word, try the list characters above in their given order. From what I've seen, it doesn't work 100% of the time, but it certainly gets it correct more often than not.




In Part II (goodness, a Part II?!?!), I plan to write a test framework that uses the dictionary I downloaded to randomly calculate the accuracy of this method. I'll use a large set of sample data, and see how often I can guess the word correctly based on the rules above without specifying ten incorrect answers.




In Part III (holy smokes!), I'd like to make the rules more intelligent by changing the most common letters used based on the known letters in the word. For instance, if the word has an S and T, I'd like to know if an E or I is more likely to also be in the word.




Chances are it'll be awhile before I get to Part II and Part III, but I think I'll get there eventually.




I truly hope that at least someone else out there has fond some value in this post! Here's the full source-code, along with all the parsed files (look in the debug directory).




[Dictionary.zip (2.15 MB)](http://images.wadewegner.com/wordpress/content/binary/Dictionary.zip)




Please let me know if you have any thoughts!
