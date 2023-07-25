---
layout: post
title:  "Cracking Complexity: How ChatGPT Unravels Obfuscated C Code from the IOCCC"
date:   2023-07-25 14:37:21 +0300
categories: AI Obfuscation Research
tags: AI obfuscation ioccc langchain ChatGPT
---
# Cracking Complexity: How ChatGPT Unravels Obfuscated C Code from the IOCCC

## Introduction

Since the launch of [ChatGPT](https://chat.openai.com/), many questions have arisen regarding its ability to write code and, more importantly, to understand it. It appears that language models (LLMs) can comprehend complex code snippets, simplify them, and optimize their performance. Recently, I came across a [thread](https://news.ycombinator.com/item?id=34503233) discussing whether ChatGPT can understand obfuscated code, specifically C code, as it seems to handle https://obfuscator.io/ (JS code obfuscator) quite well.

During my time at [NDS](https://en.wikipedia.org/wiki/Cisco_Videoscape), I worked on an in-house code obfuscator project. It was a plugin built on top of [LLVM](https://llvm.org/), adding an obfuscation layer to C/C++ code. Since then, the field of code obfuscation has always fascinated me—how to create code that computers can read but is difficult for humans to decipher, and what characteristics make code "unreadable."
The breakthrough of ChatGPT and other LLM systems raises many questions in this domain. Are they capable of understanding complex code? Can they put an end to code obfuscation techniques? What are the computational abilities of ChatGPT? Does it possess inherent computational capabilities, or is its understanding solely based on learned examples?

In this blog, I attempt to find answers to some of these questions by evaluating ChatGPT against numerous C code examples. I curated a dataset of 319 computer programs, all taken from the [IOCCC](https://www.ioccc.org/), to test if ChatGPT can comprehend each program's functionality. Subsequently, I explored specific cases that appeared intriguing for further in-depth research. 

## The IOCCC Challenge and Its Complexity
[The International Obfuscated C Code Contest (IOCCC)](https://www.ioccc.org/) has been a celebrated annual programming competition since its inception in 1984, captivating programmers worldwide with its unique challenge. Participants are tasked with crafting C programs that not only function correctly but are cleverly obfuscated, presenting a cryptic and puzzling appearance. Winning entries are judged based on their artistic and creative presentation rather than conventional functionality. This playground for ingenious programmers showcases the essence of code obfuscation, deliberately adding complexity, redundancies, and abstractions to source code to confound human understanding.

The heart of the IOCCC lies in the concept of code obfuscation, a practice with essential roles in both software development and security. Deliberate obfuscation aims to protect intellectual property, prevent reverse engineering, and deter unauthorized tampering. Decoding such obfuscated code demands an innovative and unique approach, challenging traditional methods of comprehension used by human programmers and code analysis tools alike.

The IOCCC provides a diverse array of obfuscated C code entries, each presenting unique obfuscation techniques and unconventional coding styles. This rich collection of complex and challenging code serves as an ideal dataset for evaluating ChatGPT's proficiency in deciphering intricate programming constructs and recognizing obfuscation patterns. By subjecting ChatGPT to the IOCCC's repertoire of enigmatic code snippets, this blog post seeks to explore the AI language model's abilities in tackling real-world challenges faced by developers dealing with code obfuscation.



Try to understand what the following C program does:

```c
                                           #define r(R) R"()"
                          /*[*/#include  /**/<stdio.h>
                      #include<math.h>/*!![crc=0f527cd2]*/
                   float I,bu,k,i,F,u,U,K,O;char o[5200];int
              #define R(U) (sizeof('U')==1||sizeof(U"1"[0])==1)
            h=0,t=-1,m=80,n=26,d,g,p=0,q=0,v=0,y=112,x=40;  float
           N(float/*x*/_){g=1<<30;d=-~d*1103515245&--g;return  d*_
          /g;}void/**/w(int/**/_){if(t<0){for(g=0;g<5200;o[g++   ]=
          0);for(;g;o[g+79]=10)g-=80;for(t=37;g<62;o[80+g++]=32)   ;
         }if(m&&o[h*80+m-1]==10){for(g=0;g<79;o[t*80+g++]=0){}o[t
         ++*80+g]=10;t%=64;n+=2;I=N(70)+5;if(n>30&&(I-x)*(I-x)+n*
        n>1600&&R()){O=0;F=(x=0x1!=sizeof(' '))?k=1+N(2),i=12-k+N(
        8),N(4):(k=17+N(5),i=0,r()[0]?O=.1:  0);for(u=U=-.05;u<32;
        U=k+i+i*.5*sin((u+=.05)+F))for( K=0   ;K< U;K+=.1)if((bu=K*
       sin(u/5),g=I+cos( u/5) *K)>=0&&g  <     79  )o[g+(int)(t+44+
       bu*(.5-(bu>0?3*O:  O)   ) )%64*  80      ]  =32;x*=02//* */2
      -1;n=O+x?n=I+(x?0   :N     (k)-   k           /2),g=(t+42  )%
      64,m=-~g%64,x?g=m          =-~        m%64:0  ,n>5?o[g*80   +
     n-3]=o[m*80+n-3]=       0:   0              ,n <75?o[g*80+n
     +2]=o[m*80+n+2]=0   :0:0;                      x=I;}h=-~h%64
    ;m=0;}putchar((g=o [h*                          80+m++])?g:_);
   if(g){w(_);}}void W                               (const char*_
  ){for(;*_;w(*_++));}                               int main(int a
  ,char**_){while(a--)d              +=_[a          ]-(char*)0;W( \
 "#include<stdio.h>typed"             "e"         "f\40int\40O;v"
 "oid o(O _){putchar(_);}O"                    "\40main(){O"  ""
"*_[512],**p=_,**d,b,q;for(b=0;b"        "++<512;p=_+q)_[q"    \
"=(p-_+1)*9%512]=(O*)p;") ;      for(;(g= getchar())-EOF;p=
q){q=p;for(v=512;p-q-g&&q-p-              g;  v--)q=-~q*9%512
;W("o(");if(p>q)w(y),w(45);w(                      40);w(y^=20
);w(075);for(a=0;a<v;a++)w(42);                      for(W("(O**"
 );a--;w(42)){}w(41);w(y^024);w(                      41);if(p<=q)w(
   45),w(y^20);W(");");}for(a=7;a-6                      ;W(a<6?"{;}":""
      ))for(a  =0;a  <6 &&   !o[h*80+m                       +a];a++){}W("r"
         "etu"  /*J   */       "rn+0;}\n"                             );return
             /*                      "#*/0                                   ;}
```

From IOCCC: [`yang` 2015](https://www.ioccc.org/2015/yang/prog.c)

## Building an IOCCC Database for Research
To efficiently collect the essential information for my research, I developed a Python script to retrieve and organize 
the data into an SQLite database. This streamlined approach allowed me to create a comprehensive database of IOCCC 
entries with ease.

The database includes each entry's name, year of participation, the obfuscated C program, the hint provided by the author, and the corresponding spoiler. 

For easy access and analysis, I have also shared the Python script and a prebuilt version of the SQLite database in a GitHub repository at https://github.com/nir-mo/ioccc-db. 

## Analyzing ChatGPT's Performance

In the initial phase of my research, I utilized a Python script powered by [LangChain](https://python.langchain.com/docs/get_started/introduction.html), an innovative library for interacting with OpenAI's GPT-3 language models. This script was designed to map IOCCC entries by querying the language model for insights into the functionality of the obfuscated C programs. By establishing a prompt template, the script simulated the expertise of a __master in C programming language__, asking them to explain each C program's purpose or say `I don't know` if it failed to understand.

Note that in some cases the prompt has too many tokens, __I skipped these entries__.


```python
from langchain.llms import OpenAI
from openai.error import InvalidRequestError

llm = OpenAI(openai_api_key="BLA")

import sqlite3

results = []

prompt_template = PromptTemplate(
    template = """
        You are a master in C programming language, please tell me what this program does. 
        if you can't please say 'I don't know': {c_prog}"
    """,
    input_variables=["c_prog"]
)

with sqlite3.connect('ioccc_winners.sqlite') as ioccc_winners_db:
    cursor = ioccc_winners_db.cursor()
    cursor.execute("SELECT * FROM winners")
    for row in cursor:
        name = row[0]
        year = row[1]
        spoiler = row[2]
        prog = row[3]
        try:
            result = llm(prompt_template.format(c_prog=prog))
            results.append((name, year, spoiler, result))
        except InvalidRequestError:
            results.append((name, year, spoiler, "Error: Too many tokens"))
        
```

### Filter the results - what OpenAI detected successfully



```python
success = []

for name, year, spoiler, result in results:
    result = result.strip()
    if result.startswith("Error:") or result.startswith("I don"):
        continue
    
    success.append((name, year, spoiler, result))


# Print OpenAI success rate for understanding obfuscated C programs.
success_ratio = len(success) / len(results)
success_ratio
```




    0.5454545454545454



In the evaluation of IOCCC entries, __ChatGPT initially demonstrates a success rate of around 54%__. 

__However__, a closer examination of the results highlights a significant number of __false positives__. 

For example, when presented with the `westley/1988` program, ChatGPT incorrectly interprets its purpose as performing an undefined division operation (`"This program prints the result of 4 multiplied by minus one divided by zero divided by zero, which is undefined."`), rather than accurately identifying its intended functionality of calculating the value of Pi, as intended by the program's writer. 

Addressing these false positives becomes the next crucial step to achieve a more accurate success rate. By carefully verifying and refining ChatGPT's responses, we aim to improve its ability to comprehend the complexities of various obfuscated C code entries, ensuring more precise interpretations and evaluations moving forward.


```python
import pandas as pd

df = pd.DataFrame(results, columns=['Name', 'Year', 'Spoiler', 'Result'])

westley = df[(df.Year == 1988) & (df.Name == 'westley')]

print(f"The actual spoiler (calaulate Pie) = {westley['Spoiler'].values[0]}")
print(f"OpenAI analysis = {westley['Result'].values[0].strip()}")
```

    The actual spoiler (calaulate Pie) = prints '3.141', circle made of '_-_-_-_' in layout
    OpenAI analysis = This program prints the result of 4 multiplied by minus one divided by zero divided by zero, which is undefined.


### ChatGPT Assessing its Own Analysis of IOCCC Entries

The provided Python code snippet serves the purpose of verifying the accuracy of ChatGPT's analysis of IOCCC entries by leveraging the language model's capabilities to determine if its responses align with the clues provided by the program writers. To achieve this, the script employs a new prompt template, `prompt_template_check_similarity`, which compares the expert analysis generated by ChatGPT with the corresponding spoilers from the IOCCC database.

The script iterates through the successful responses obtained earlier, where ChatGPT attempted to interpret the functionality of various obfuscated C programs. For each entry, the script prompts ChatGPT to compare its own expert analysis with the spoiler given by the program writer. By asking ChatGPT to decide if its analysis was `Close enough` or `Wrong!` in relation to the writer's clue, the code seeks to identify any discrepancies or false positives in the initial responses.

The intriguing aspect of this code lies in the fact that ChatGPT is tasked with verifying its own accuracy, providing an additional layer of evaluation and refinement. The results obtained from this verification process, stored in the `accurate_results` list, enable researchers to identify areas where ChatGPT's performance can be improved and where it aligns well with the intentions of the program writers. This validation step is valuable in fine-tuning ChatGPT's comprehension of complex and cryptic code, ultimately leading to more precise and reliable analysis in the context of the IOCCC entries.


```python
prompt_template_check_similarity = PromptTemplate(
    template = """
    I asked a C-language expert to analyze all the C programs from the IOCCC. 
    I have some clue about the purpose of each program (from the program's writer).
    
    You are a program which helps me to determine if the C expert was right, close enough or completely wrong 
    regarding his analysis. 
    I'm giving you the C-expert analysis and also the clue from the program writer, please help me to determine 
    if the C expert was right.
    If the C expert was correct or close enough or even understand the main logic then say "Close enough" 
    otherwise say "Wrong!".
    
    Expert analysis:
    ```
    {expert_analysis}
    ```
    
    
    The clue:
    ```
    {spoiler}
    ```

    """,
    input_variables=["expert_analysis", "spoiler"]
)

accurate_resutls = []

for name, year, spoiler, result in success:
    similarity_result = llm(prompt_template_check_similarity.format(expert_analysis=result, spoiler=spoiler))
    accurate_resutls.append((name, year, spoiler, result, similarity_result.strip()))

```

After implementing measures to limit false positives, we reevaluated ChatGPT's success rate in analyzing IOCCC entries. 


```python
false_positives = [
    was_chat_gpt_wrong 
    for _, _, _, _, was_chat_gpt_wrong in accurate_resutls 
    if was_chat_gpt_wrong == "Wrong!"
]

print(f"Success rate = {(len(accurate_resutls) - len(false_positives)) / len(results)}")
```

    Success rate = 0.25391849529780564



__After eliminating false positives, ChatGPT's success rate in analyzing IOCCC entries is observed to be approximately 25%__.

## Insights and Revelations: ChatGPT's Training Clues and Struggle with Short, Deterministic Code


In this section, two intriguing case studies come to light. The first focuses on ChatGPT's analysis of `endoh2/2013`. When prompted to discern the program's purpose, ChatGPT responded `"This program is an implementation of an International Obfuscated C Code Contest (IOCCC) challenge from 2013. It reads an input string from the keyboard and prints out a series of characters based on the input string."`. 

This astute recognition of the IOCCC challenge reveals a significant clue that ChatGPT was indeed trained on data encompassing obfuscated entries, making it familiar with the complexities and patterns of the IOCCC codebase.


```python
endoh2 = df[(df.Year == 2013) & (df.Name == 'endoh2')]

print(endoh2['Result'].values[0].strip())
```

    This program is an implementation of an International Obfuscated C Code Contest (IOCCC) challenge from 2013. It reads an input string from the keyboard and prints out a series of characters based on the input string.


Intriguingly, another compelling case study is the examination of `westley/1988` a relatively short and deterministic program that calculates Pi. 

```c
#define _ -F<00||--F-OO--;
int F=00,OO=00;main(){F_OO();printf("%1.3f\n",4.*-F/OO/OO);}F_OO()
{
            _-_-_-_
       _-_-_-_-_-_-_-_-_
    _-_-_-_-_-_-_-_-_-_-_-_
  _-_-_-_-_-_-_-_-_-_-_-_-_-_
 _-_-_-_-_-_-_-_-_-_-_-_-_-_-_
 _-_-_-_-_-_-_-_-_-_-_-_-_-_-_
_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_
_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_
_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_
_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_
 _-_-_-_-_-_-_-_-_-_-_-_-_-_-_
 _-_-_-_-_-_-_-_-_-_-_-_-_-_-_
  _-_-_-_-_-_-_-_-_-_-_-_-_-_
    _-_-_-_-_-_-_-_-_-_-_-_
        _-_-_-_-_-_-_-_
            _-_-_-_
}
```
From IOCCC [westley/1988](https://www.ioccc.org/1988/westley.c)

While this program's analysis lies outside the scope of this blog post, it serves as a perplexing example. With no inputs and no random variables, and given its deterministic nature, it was expected that ChatGPT would comprehend its purpose. However, surprisingly, ChatGPT completely failed to understand this program. The inability to grasp such a straightforward and constant expression raises questions about ChatGPT's computational abilities in handling deterministic calculations, even those that could be evaluated at compilation time. 


## Implications and Future Directions

The exploration of ChatGPT's performance in deciphering obfuscated C code from the IOCCC has uncovered intriguing insights and potential directions for further research. To shed light on the implications of this study and pave the way for future investigations, several promising avenues emerge:


__Explore more real-world obfuscations__:
One important consideration regarding the current dataset is its public availability, which raises the possibility that ChatGPT may have been trained on these programs. To expand the scope of the research and avoid potential biases, it is crucial to seek additional code examples that are not as widely accessible. Furthermore, while the IOCCC entries present captivating and artistic obfuscation challenges, they may not fully represent the practical obfuscation techniques found in real-world scenarios. By incorporating more diverse and less publicly available code samples, we can better evaluate ChatGPT's ability to comprehend a wider range of obfuscation styles encountered in practical software development and security contexts.

__Comparing the Results Against Other LLMs__:
An exciting next step involves comparing ChatGPT's performance with other Language Model-based approaches (LLMs) in understanding obfuscated C code. By evaluating different LLMs' capabilities and identifying their respective strengths and weaknesses, researchers can gain a comprehensive understanding of the state-of-the-art in code comprehension. Such comparisons may highlight unique features of individual models, offering valuable insights to improve the overall effectiveness of AI-driven code analysis.

__Trying to Write a Candidate for IOCCC Using ChatGPT__:
In the spirit of the IOCCC challenge, attempting to generate an entry using ChatGPT as a candidate could be an intriguing endeavor. Leveraging ChatGPT's language generation capabilities, developers can explore the creative boundaries of code obfuscation, providing a fascinating experiment in AI-driven code creation. This exercise may unveil new obfuscation techniques and offer an opportunity to evaluate ChatGPT's proficiency in crafting complex and cryptic C programs.

__Providing Simple Tools to Improve Results__:
As ChatGPT navigates the complexities of obfuscated C code, providing supplementary tools and agents can be instrumental in enhancing its performance. Simple tools like a C-Preprocessor or a C beautify tool can help streamline code analysis by reducing redundancies, optimizing code readability, and potentially aiding ChatGPT in better understanding the underlying logic. These auxiliary tools can act as valuable aids, empowering ChatGPT to navigate intricate code constructs with increased accuracy.



## Conclusion 

In conclusion, this research has shed light on ChatGPT's proficiency in analyzing obfuscated C code from the IOCCC. However, it is essential to recognize that ChatGPT is primarily a natural language model, lacking real computational abilities. While it can excel in understanding certain programming constructs based on its training data, its performance is bound by the information it was exposed to during training.

The study highlights the crucial distinction between natural language comprehension and genuine computational capabilities. Despite ChatGPT's language understanding prowess, it relies heavily on the patterns and knowledge present in the data it was trained on. This inherent limitation underscores the need for caution when interpreting ChatGPT's responses, especially in intricate scenarios such as code obfuscation.

The research reaffirms that code obfuscation remains an enduring challenge, and the quest for new and inventive techniques persists. As we embrace the potential of AI language models, we must also acknowledge their inherent constraints and remember that genuine computational abilities require innovative approaches beyond the scope of natural language comprehension.


If you have any questions, suggestions, or would like to discuss further insights from this research, please feel free to contact me. I welcome your inquiries and look forward to engaging in meaningful discussions about code comprehension, AI language models, and the fascinating world of obfuscated C code.
