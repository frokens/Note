## 抽象类：manus
继承[[ToolCallAgent]]
### attribute：
1. name: str
2. description: str
3. prompt (用于生成planning 和 下一步的action)
	- system_prompt: str
	- next_step_prompt: str
4. max(超出限制退出系统)
	- max_observe: int 
	- max_step: int
5. available_tools: ToolCollection
	1. PythonExeute( )
	2. WebSearch( )
	3. BrowserUseTool( )
	4. FileSaver( )
	5. Terminate( )
## function:
1. *async* _handle_special_tool(self, name: str, result: Any, **kwargs)
2. 
