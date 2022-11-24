## Scratchpad UI
The **Scratchpad UI** functions like the **Ace UI** (hopefully a familair face by now), except there are two spots to enter text. The first editor is identical to the **Ace UI**. The second editor can be shown by clicking the **Scratchpad** button, this also includes a **Run Button** and a **Prefix with Answer Checkbox**. The second editor is independant: code entered is not included when marking submissions. Students can use the UI in one of two ways:
 - When prefix with answer is not checked, only the code in the scratchpad is run -- allowing for a rough working spot to quickly check the result of code.
 - When the prefix anser is checked, the code in the scratchpad is appended to the end of their answer -- allowing testing in-browser, without using check.
 
 The names of all buttons are changable via UI paramiters.
 
 By defualt, clicking Run will provide an escaped textual output. In some cases, e.g. programming HTML, this is not very useful. You can allow HTML output by setting `sp_html_out` to `true`.

### Advanced Use: Wrappers
Sometimes this defualt configuration won't be flexible enough... What if you want use a language installed on jobe but not supported by codeurnner, or to display Matplotlib graphs using Run? This is where you'd use a wrapper to do your bidding.

A wrapper is a program used to 'wrap' your code up before it gets run using the sandbox. You can write a program to insert the answer code and scratchpad code into, using `{{ Student_Code* }}` and `{{ Scratchpad_Code }}` respectively. When the prefifix checkbox is unchecked `{{ Student_Code* }}` is replaced with the empty string `''`.  For example, the defualt configuration can be thought of as the following wrapper:
```
{{ Student_Code* }}
{{ Scratchpad_Code }}
```
*included only if prefix ticked, otherwise empty string

To have some fun, why not make the scratchpad code repeat ten times per Run using python...
```
{{ Student_Code }}
for i in range(10):
    exec('''{{ Scratchpad_Code }}''')
```

 You can set the Run language, using `sp_run_lang`, this changes the language sent to the sandbox service used to run the wrapper. This means you can write your wrapper in a different language to the question. To the person answering the question this would be invisible. Below is an example of how you could run C using Python as your run language (see the multi-language question for further insparation).
 ```
 import subprocess
 
student_answer = """{{ Student_Code }}"""
test_code = """{{ Scratchpad_Code }}"""
all_code = student_answer + '\n' + test_code
 filename = '__tester__.c
 with open(filename, "w") as src:
    print(all_code, file=src)

cflags = "-std=c99 -Wall -Werror"
return_code = subprocess.call("gcc {0} -o __tester__ __tester__.c".format(cflags).split())
if return_code != 0:
    raise Exception("** Compilation failed. Testing aborted **")
exec_command = ["./__tester__"]
 
 output = subprocess.check_output(exec_command, universal_newlines=True)
print(output)
 ```
To expand on this more, you could wrap the scratchpad code inside a main function with the ansewr code outside, this might be useful for the first questions in a C course -- it would remove one thing for a beginer to remember.

Ealier, the `sp_html_out` paramiter was discussed. In conjunction with a wrapper this can be used to display graphical/non-textual output in the output box. If we allow HTML output, then write a wrapper that runs the code and grabs the image, printing it as a data URI inside an HTML `<img>` tag.

 

### Params Summary

- `sp_name`: the display name of the scratchpad
- `sp_button_name`: the run button text to display
- `sp_prefix_name`: the prefix answer check text to display
- `sp_ace_lang`: the language used inside in the scratchpad when answering the question, this controlls syntax highlighting for
- `sp_run_lang`: the langauge used to run code when the run button is clicked, this should be the langauge your wrapper is written in (if applicable)
- `sp_run_wrapper`: 
- `sp_run_wrapper_globalextra`: set to `true` if you want 
- `sp_html_out`: when true, the output from run will be raw HTML instead of textual
