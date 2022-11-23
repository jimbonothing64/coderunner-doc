## Scratchpad UI
The **Scratchpad UI** functions like the **Ace UI** (hopefully a familair face by now), except there are two spots to enter text. The first editor is identical to the **Ace UI**. The second editor can be shown by clicking the **Scratchpad** button, this also includes a **Run Button** and a **Prefix with Answer Checkbox**. The second editor is independant: code entered is not included when marking submissions. Students can use the UI in one of two ways:
 - When prefix with answer is not checked, only the code in the scratchpad is run
 - When the prefix anser is checked, the code in the scratchpad is appended to the end of their answer, allowing testing in-browser -- without using check.
 
 By defualt, clicking Run will provide an escaped textual output. In some cases, e.g. programming HTML, this is not very useful. You can allow HTML output by setting `sp_html_out` to `true`.

 Another case



 *included if prefix ticked, otherwise empty string
 

### Params Summary

- `sp_name`: the display name of the scratchpad
- `sp_button_name`: the run button text to display
- `sp_prefix_name`: the prefix answer check text to display
- `sp_ace_lang`: the language used inside in the scratchpad when answering the question, this controlls syntax highlighting for
- `sp_run_lang`: the langauge used to run code when the run button is clicked, this should be the langauge your wrapper is written in (if applicable)
- `sp_run_wrapper`: 
- `sp_run_wrapper_globalextra`:
- `sp_html_out`: when true, the output from run will be raw HTML instead of textual

To edit,