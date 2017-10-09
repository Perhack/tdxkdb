# tdxkdb
call kdb+/q from TDX formulas

## tdxkdb功能 

    实现tdx(<http://www.tdx.com.cn>)的dll公式，可以将tdx数据传到kdb计算并返回计算结果，可以实现复杂的计算。

## 使用方法 

1、下载安装kdb+/q。把qfml.q提制到q目录。根据需要修改qfml.q里的这些函数：.fml.tdxkdb1、.fml.tdxkdb2、.fml.tdxkdb3、.fml.tdxkdb4、.fml.tdxkdb5。可能用到一个或多个，最多只能用5个。这些函数有三个参数，参数值来自dll公式中的参数序列，返回结果必须与参数序列长度相同，类型为`float或`real。

2、启动q qfml.q -p 5001 -U qusers。 如果有用户验证(q qfml.q -p 5001 -U qusers)，需要在用户密码文件（如qusers文件）中添加用户密码：fml:input_fml_pwd（这是固定的）。  【5001为q端口，还可以是5002,5003,5004,5005】

3、把c.dll复制到tdx主目录（如d:\tdx），不是\tdx\T002\dlls目录！！！

4、把tdxkdb5001.dll复制到\tdx\T002\dlls目录!!!。 【如果q端口是5002,5003,5004,5005，则分别复制tdxkdb5002.dll、tdxkdb5003.dll、tdxkdb5004.dll、tdxkdb5005.dll】

5、打开TDX公式管理器(Ctrl+F)，点“DLL函数”，选择需要绑定的DLL，假设为“第1号DLL”，“打开绑定”，选择\tdx\T002\dlls下的tdxkdb5001.dll，确定后显示“成功绑定了tdxkdb5001.dll”。如果绑定失败，请检查c.dll是否在tdx主目录中！   【如果q端口是5002,5003,5004,5005，则绑定tdxkdb5002.dll、tdxkdb5003.dll、tdxkdb5004.dll、tdxkdb5005.dll】

6、在tdx中建立一公式，假设公式名称为KDB，公式如：
```
R1:TDXDLL1(1,O,O,O);
R2:TDXDLL1(2,O,O,H); 
R3:TDXDLL1(3,H,L,L);
R4:TDXDLL1(4,H,L,C);
R5:TDXDLL1(5,H,L,C+2);
{TDXDLL1表示第1号DLL，TDXDLL1第1个参数表示函数编号，这里只能是1、2、3、4、5，分别表示将调用.fml.tdxkdb1、.fml.tdxkdb2、.fml.tdxkdb3、.fml.tdxkdb4、.fml.tdxkdb5等kdb+/q函数，并将TDXDLL1第2、3、4个参数传给它们，kdb+/q函数传回同长度的序列值，即为TDXDLL1函数返回值。例如TDXDLL1(3,H,L,C)表示返回.fml.tdxkdb3[H;L;C]的执行结果。}
```

7、其它注意事项：

(1)如果dll函数返回值为-99，说明dll公式连接kdb+/q失败，请检查：1）q的端口和绑字的文件是否匹配，如端口为5002，则绑定文件应为jztkdb5002.dll，这是写到dll里的； 

(2)访问q是否需要用户名密码验证，如果需要，请使用fml:input_fml_pwd，这是写到dll里面的，无法修改。

(3)如果dll函数返回值为-999，说明.fml.tdxkdb1等函数时出错，请检查函数是否正确，并能返回`float或`real序列，同时序列长度正确。

(4)tdx的DATE、TIME、TIME2函数的值转为q的date、time、time的函数：.fml.tdxdate2qdate,.fml.tdxtime2qtime,.fml.tdxtime22qtime，q的date、time、time转为tdx的DATE、TIME、TIME2的函数：.fml.qdate2tdxdate、.fml.qtime2tdxtime、.fml.qtime2tdxtime2。
