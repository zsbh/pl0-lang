<!--
 * @Author: zhangsunbaohong
 * @Email: zhangsunbaohong@163.com
 * @Date: 2021-10-12 07:59:47
 * @LastEditTime: 2021-10-19 22:15:09
 * @Description:
-->

# pl0-lang

这是使用 C++写的 pl0 语言编译器

## 语法

```
program = block "." ;

block = [ "const" ident "=" number {"," ident "=" number} ";"]
        [ "var" ident {"," ident} ";"]
        { "procedure" ident ";" block ";" } statement ;

statement = [ ident ":=" expression | "call" ident
              | "?" ident | "!" expression
              | "begin" statement {";" statement } "end"
              | "if" condition "then" statement
              | "while" condition "do" statement ];

condition = "odd" expression |
            expression ("="|"#"|"<"|"<="|">"|">=") expression ;

expression = [ "+"|"-"] term { ("+"|"-") term};

term = factor {("*"|"/") factor};

factor = ident | number | "(" expression ")";
```

## 设计

pl0 是一个类 pascal 语言, 该编译器使用 C++语言完成，从词法解析，语法解析到 AST，直至 JIT 执行结束

### [1. Lexer](./docs/Lexer.md)

## 规范

[使用 Google 代码风格](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/naming/#general-naming-rules)

## 用法

本项目使用[vcpkg](https://vcpkg.io/en/index.html)作依赖管理，使用 cmake 进行构建
安装 vcpkg，vscode 配置 CMAKE_TOOLCHAIN_FILE=/install_path/vcpkg/scripts/buildsystems/vcpkg.

- mkdir build
- cd build
- cmake ..
- make
- ./pl0 "filepath"
- "exec the program"
- make clean

## LICENSE

MIT © [duduscript](https://github.com/duduscript)

```
# 词法解析
# in: stream
# out: list<token>

parser(in:stream) {
	result = list<token>();
	while(in.peek() != EOF) {
		if (match(in, 'alpha')) {
			result.push_back(getWord(in));
		} else if (match(in, 'number')) {
			result.push_back(getNumber(in));
		}
		}
	}
	return result;
}

result = [{value = 'if', type='word'}, {value='23.2', type='number'}]

// 缺少；号属于语法解析是报错
// 9432nhc不属于标识符报错，词法解析报错
// pl0使用括号处理块作用域，将无异议空白字符在词法解析时处理掉，\n处理完后将相关属性放在token里
```
