#为什么不用eval
##MDN和StackOverFlow上都已经给出了答案  
  
* MDN <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval>
* StackOverflow
<http://stackoverflow.com/questions/86513/why-is-using-the-javascript-eval-function-a-bad-idea>  

---
####总体来说分以下几点：  
1. 运行慢。eval执行其中代码时，调用js的解释器，而其他的JS代码已经被浏览器引擎优化了  
2. 安全性。恶意代码可以直接注入进来执行
3. 调试不方便。无法打印其中代码日志，无法跟踪断点
4. eval中的代码易出问题，编码不方便