---
layout: post
title: Blog with Github Pages
date: 2014-09-12 10:45:00
category: Database
---

## Oracle Text: The Word List and the WILDCARD_MAXTERMS attribute
-----
As data grow more and more, an error of WILDCARD_MAXTERMS expansion meets eventually. What's wildcard_maxterms? How does it work?

Here we will have a briefly description and explaination:

> WILDCARD_MAXTERMS is a property of BASIC_WORDLIST, it specifies the maximum number of terms in a wildcard expansion.
> - 10g allows set WILDCARD_MAXTERMS to maximum as 15000, default value is 5000
> - 11g allows set WILDCARD_MAXTERMS to maximum as 50000, default value is 20000

You can set value of WILDCARD_MAXTERMS like this:
<code>
execute ctd_ddl.drop_preference ('my_wordlist'); 
BEGIN   
    ctx_ddl.create_preference ('my_wordlist', 'BASIC_WORDLIST'); 
    ctx_ddl.set_attribute ('my_wordlist', 'WILDCARD_MAXTERMS', 20000); 
END; 
/
</code>

But, if the error still thrown even reach the maximum, how we can do?
 1. Narrow your query string, match more exact expected result
 2. Make the lexer more smarter to consider specific characters as normal alphanumberic characters, such as slash, this requires modifying in parameter `PRINTJIONS`:
<code>
begin
ctx_ddl.create_preference('OBJECT_LEXER', 'BASIC_LEXER');
ctx_ddl.set_attribute('OBJECT_LEXER', 'printjoins', '_-~*''@#%^&.()+=:";');
end;
/
</code>
