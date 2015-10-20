---
layout: post
title:  "What I Learnt Today-20-10-2015"
date:   2015-10-20 
desc: "Today with my fresh brain i started solving letter change puzzle. it was really challenging and learnt many things"
---

####What I Learnt Today
Today with my fresh brain i started solving letter change puzzle. it was really challenging and learnt many things.

###puzzle:
Have the function LetterChanges(str) take the str parameter being passed and modify it using the following algorithm. Replace every letter in the string with the letter following it in the alphabet (ie. c becomes d, z becomes a). Then capitalize every vowel in this new string (a, e, i, o, u) and finally return this modified string.

{%highlight ruby%}
def LetterChanges(str)
   alph = %w{a b c d e f g h i j k l m n o p q r s t u v}
   vowel = 'aeiou'
   str = str.split('')
   str.each_with_index do |x, str_index|
      alph.each_with_index do |alp, alph_index|
         if (alp.eql? x)
            str[str_index] = alph[alph_index+1] 
            break
         end
      end
      str[str_index].upcase! if (vowel.include? str[str_index])
   end
   # code goes here
   return str.join 
         
end
   
# keep this function call here 
# to see how to enter arguments in Ruby scroll down   
puts LetterChanges(STDIN.gets)  

{%endhighlight%}

Note:

ord() - returns the code point of the character.
'a'.ord - 97
