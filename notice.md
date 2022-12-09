## php笔记
---
### 类型转换
*	优先级（浮点型>整型>字符串）
*	强制类型转换及类型
```
-	(int),(integer)  -- 转换为整型 integer
-	(bool),(boolean)  --  转换为布尔类型 Boolean
-	(float),(double),(real)  --  转换为浮点型 float
-	(string)  --  转换为字符串 string
-	(array)  --  转换为数组 array
-	(object)  --  转换为对象 object
-	(unset)  -- 转换为NULL（PHP 5）
```
*	字符串
	- 	字符串是 PHP 中重要的数据类型之一。在 Web 开发中，很多情况下都需要对字符串进行处理和分析，通常将涉及字符串的格式化、字符串的连接与分割、字符串的比较、查找等一系列操作。用户和系统的交互也基本上是用文字来进行的，因此系统对文本信息，即字符串的处理非常重要。
	- 	PHP 中提供了大量用来处理字符串的内置函数，使用这些函数，可以在 PHP 程序中很方便地完成对字符串的各种操作。
	- 	预定义函数对于字符串的大小写转换
		+	strtoupper()，将字符串转换为大写
		+	strtolower()，将字符串转换为小写
		+	ucfirst()，将字符串首字母转换为大写
		+	lcfirst()，将字符串首字母转换为小写
		+	ucwords()，将字符串首个单词转换为大写
		+	mb_strtoupper()，将字符串转化为大写，可以设置参数的字符编码（与 strtoupper 函数有区别
		+	mb_strtolower()，将字符串转化为小写，可以设置参数的字符编码（与 strtolower 函数有区别）
		+	mb_convert_case()，按照不同的模式将字符串进行转换，$str 是需要转换的字符串；$mode 是转换模式，可以是 MB_CASE_UPPER、 MB_CASE_LOWER 和 MB_CASE_TITLE 的其中一个；$encoding 是参数的字符编码，可以省略。
	- 	字符串查找
		+	stripos()，用来查找字符串中某部分字符串首次出现的位置（不区分大小写）。
		```
		// stripos(string $haystack, string $needle [, int $offset = 0])
		// haystack：在该字符串中查找。
		// needle：needle 可以是一个单字符或者多字符的字符串。如果 needle 不是一个字符串，那么它将被转换为整型并被视为字符顺序值。
		// offset：可选的 offset 参数允许你指定从 haystack 中的哪个字符开始查找，返回的位置数字值仍然相对于 haystack 的起始位置。
		// 返回 needle 存在于 haystack 字符串开始的位置（独立于偏移量）。同时注意字符串位置起始于 0，而不是 1。如果未发现 needle 就将返回 false。
		```
		+	strpos()，查找子串在字符串中首次出现的位置，区分大小写
		+	strripos()，查找子串在字符串中最后一次出现的位置，不区分大小写
		+	strrpos()，查找子串在字符串中最后一次出现的位置，区分大小写
		```
		// 字符串查找
		$str = "Where is the letter 'A' or 'a' in this sentences?Show the a letter 'A'.";
		$helloStr = "Hello, World.";
		$targetStr = 'a';
		// stripos()，查找子串首次在字符串中出现的下标，不区分大小写
		$offset = stripos($str, $targetStr);
		$offset1 = stripos($helloStr, $targetStr);
		var_dump($offset);
		var_dump($offset1);
		// stripos()，查找子串首次在字符串中出现的下标，区分大小写
		var_dump(strpos($str, $targetStr));
		// strripos()，查找子串最后一次在字符串中出现的下标，不区分大小写
		var_dump(strripos($str, $targetStr));
		// strrpos()，查找子串最后一次在字符串中出现的下标，区分大小写
		var_dump(strrpos($str, $targetStr));
		```
	- 	字符串替换
		+	str_replace()，使用子串替换字符串中指定的目标字符串，不区分大小写
		+	str_ireplace()，使用子串替换字符串中指定的目标字符串，区分大小写
		```
		mixed str_ireplace(mixed $search, mixed $replace, mixed $subject [, int &$count])
		该函数返回一个字符串或者数组。该字符串或数组是将 subject 中全部的 search 用 replace 替换（忽略大小写）之后的结果。参数 count 表示执行替换的次数。
		```
		+	substr_replace()，用子串从指定下标区间的字符串中替换目标字符串
		```
		mixed substr_replace(mixed $string, mixed $replacement, mixed $start [, mixed $length]),
		substr_replace() 在字符串 string 的副本中将由 start 和可选的 length 参数限定的子字符串使用 replacement 进行替换。
		如果 start 为正数，替换将从 string 的 start 位置开始。如果 start 为负数，替换将从 string 的倒数第 start 个位置开始。
		如果设定了 length 参数并且为正数，就表示 string 中被替换的子字符串的长度。如果设定为负数，就表示待替换的子字符串结尾处距离 string 末端的字符个数。如果没有提供此参数，那么默认为 strlen(string)（字符串的长度）。当然，如果 length 为 0，那么这个函数的功能为将 replacement 插入 string 的 start 位置处。
		```
		```
		$str = 'hello, world!Hello world.';
		$targetStr = 'hello';
		$replaceStr = 'hi';
		echo str_ireplace($targetStr, $repalceStr, $str, 2).PHP_EOL;
		echo substr_replace($str, $replaceStr, 0, 5).PHP_EOL;
		```
	- 	字符串截取
		+	substr()，从指定下标开始截取字符串并返回子串（对英文处理）
		```
		substr(mixed $str, int $start [, int $length]);
		$string：需要截取的字符串，该字符串至少含有一个字符；
		$start：截取字符串的起始位置；
		如果 $start 是非负数，那么字符串将从 $string 的第 $start 个字符处开始截取，$start 从 0 开始计算。例如在字符串“abcdef”中，在 0 位置的字符是“a”，在 2 位置的字符串是 “c” 等等；
		如果 $start 是负数，那么字符串将从 $string 结尾处向前数第 $start 个字符开始，$start 从 -1 开始计算。例如在字符串“abcdef”中，在 -1 位置的字符是“f”，在 -3 位置的字符是“d”等等；
		如果 $string 的长度小于 $start，将返回 FALSE。
		$length：可选参数，表示截取字符串的长度。
		如果 $length 为正数，那么字符串将从 $start 位置向后截取最多 $length 个字符；
		如果 $length 为负数，那么 $string 末尾的 $length 个字符将会被省略（若 $start 是负数则从字符串尾部算起）；
		如果 $length 的值为 0，FALSE 或 NULL，那么将返回一个空字符串；
		如果没有提供 $length，那么返回的子字符串将从 $start 位置开始直到字符串的结尾。
		```
		```
		$str = 'Hello, world!hello, World.';
		echo substr($str, 13).PHP_EOL;
		echo substr($str, -13, -6).PHP_EOL;
		echo substr($str, 6, -13).PHP_EOL;
		```
		+	mb_substr()，从指定下标开始截取字符串并返回子串（对中文处理）
		```
		mb_substr(mixed $str, int $start [, int $length, string $encoding = mb_internal_encoding()]);
		$str：待截取的字符串，字符串中至少包含一个字符；
		$start：截取字符串的起始位置；
		如果 $start 为非负数，那么字符串会从 $str 的第 $start 个字符的位置开始截取；
		如果 $start 是负数，那么字符串会从 $str 结尾处向前数第 $start 个字符的位置开始截取。
		$length：可选参数，表示截取字符串的长度；
		如果 $length 为正数，那么字符串将从 $start 位置向后截取最多 $length 个字符；
		如果 $length 为负数，那么 $string 末尾的 $length 个字符将会被省略（若 $start 是负数则从字符串尾部算起）；
		如果 $length 的值 NULL 或者省略 $length，则会截取到字符串的末尾。
		$encoding：可选参数，表示 $str 的字符编码，如果省略，则使用内部字符编码。
		```
		```
		$str = '你好，世界。这是我的第一个PHP程序。';
		echo mb_substr($str, 0, 5).PHP_EOL;
		echo mb_substr($str, 8, -1).PHP_EOL;
		if(strlen($str) > 15) {
		    echo mb_substr($str, 0, 15).'...'.PHP_EOL;
		}
		else {
		    echo $str.PHP_EOL;
		}
		```
	- 	去除字符串两边的空格
		+	trim()，去除字符串左右两边的空白字符和特殊字符；
		+	ltrim()，去除字符串左边的空白字符和特殊字符；
		+	rtrim()，去除字符串左边的空白字符和特殊字符；
		```
		trim($str [, $character_mask = " \t\n\r\0\x0B"])
		$str：待处理的字符串；
		$character_mask：可选参数，用于指定所有要去除的字符，也可以使用“..”列出一个字符范围。

		如果不指定 $character_mask 参数，trim() 函数将去除下面这些字符：
		" "：普通空格符；
		"\t"：制表符；
		"\n"：换行符；
		"\r"：回车符；
		"\0"：空字节符；
		"\x0B"：垂直制表符。
		```
	- 	计算字符串长度
		+	strlen()，计算字符串长度，在 strlen() 函数中，数字、英文、小数点、下划线和空格占一个字符长度；而一个 GB2312 编码的汉字占两个字符长度，一个 UTF-8 编码的汉字占三个字符长度。
		+	mb_strlen()`mb_strlen(string $str [, $encoding = mb_internal_encoding()])`，与 strlen() 函数不同，在 mb_strlen() 函数中，无论是汉字，还是英文、数字、小数点、下划线和空格，都只占一个字符长度。
		+	提示：mb_strlen 并不是 PHP 的核心函数，使用前需要确保在 php.ini 中加载了 php_mbstring.dll，即确保“extension=php_mbstring.dll”这一行存在并且没有被注释掉，否则会出现未定义函数的问题。
		```
		$str = 'There is my first PHP program.这是我的第一个PHP程序。';
		echo 'Length of this str is : '.strlen($str).PHP_EOL;
		echo 'str的长度为：'.mb_strlen($str).PHP_EOL;
		```
	- 	字符串的转义和还原
		+	在 PHP 编程的过程中，经常会遇到这样的问题，将数据插入到数据库时可能引起一些问题，比如出现错误或者乱码等。这是因为数据库将传入的数据中的字符解释成控制符。针对这种问题，就需要使用一种标记或者是转义这些特殊的字符。
		+	在 PHP 中提供了专门处理这些问题的技术，转义和还原字符串的方法有两种，一种是手动转义、还原字符串，另一种是自动转义、还原字符串。
		+	手动转义字符串，在特殊字符前面加\\
		+	addslashes($str)，用于自动转义字符串。
		+	stripslashes($str)，用于还原字符串。
		```
		$query = "select * form test_table where id = 'slayer';";
		$testStr =addslashes($query);
		echo $testStr.PHP_EOL;
		echo stripslashes($testStr).PHP_EOL;
		```
	- 	重复一个字符串
		+	str_repeat()，重复一个字符串多少次。`str_repeat(string $str, int $num);`
	- 	随机打乱字符串
		+	str_shuffle()，随机打乱一个字符。`str_shuffle(string $str)`
	- 	字符串转数组
		+	explode()，将字符串转为数组。
		```
		explode(string $delimiter, string $str [, int $num]);
		$delimiter：用于分割字符串的分隔字符；
		$string：需要分割的字符串；
		$limit：可选参数，可以为空，规定要返回数组元素的数目；
		如果 $limit 不为空且为正数，则返回的数组最多包含 $limit 个元素，最后的那个元素包含了 $string 的剩余部分；
		如果 $limit 不为空且为负数，则返回除了最后的 $limit 个元素外的所有元素；
		如果 $limit 为 0，则会被当作 1；
		如果 $limit 为空，则表示返回所有数组元素。
		```
*	数组
	- 	数组就是一组数据的集合，把一系列数据组织起来，形成一个可操作的整体。PHP 中的数组较为复杂，但比其他许多高级语言中的数组更为灵活。
	- 	数组 array 是一组有序的变量，其中每个值被称为一个元素。每个元素由一个特殊的标识符来区分，这个标识符称为键（也称为下标）。
	- 	数组中的每个实体都包含两项，分别是键（key）和值（value）。可以通过键值来获取相应的数组元素，这些键可以是数值键，也可以是关联键。如果说变量是存储单个值的容器，那么数组就是存储多个值的容器。
	- 	数组类型
		+	索引数组，以数字作为键名的称为索引数组
		+	关联数组，以字符串或者字符串、数字混合为键名的称为关联数组
		+	关联数组的键名可以是任何一个整数或字符串。如果键名是一个字符串，则要给这个键名加上一个定界修饰符——单引号' '或双引号" "。对于索引数组，为了避免混清，最好也加上定界符。
	- 	PHP 支持一维和多维数组，可以由用户创建，也可以由一些特定的数据库处理函数从数据库查询中生成数组，以及一些函数返回数组。
	- 	在 PHP 中声明数组可以使用以下两种方法：直接为数组元素赋值即可声明数组；使用 array() 函数声明数组。
	- 	声明一个索引数组时，如果索引值是递增的，我们也可以不在方括号内指定具体的索引值，这时索引值默认从 0 开始依次增加。
	- 	声明数组的另一种方法是使用 array() 函数来新建一个数组。它接受一定数量用逗号分隔的key=>value参数对。
	```
	$array[0] = 'one';
	$array[1] = 'two';
	$array[3] = 'three';
	$array[] = 'four';
	var_dump($array);
	// php数组声明
	$array[0] = 'zero';
	$array[1] = 'one';
	$array[2] = 'two';
	$array[] = 'three';
	var_dump($array);
	$arr = [1, 2, 3, 4];
	var_dump($arr);
	$testArr = array(3, 4);
	$testArr1 = array('zero' => '0', 'one' => '1', 'two' => '2');
	var_dump($testArr);
	var_dump($testArr1);
	```
	- 	PHP 中的数组可以存储所有类型的数据，当然也包括数组本身。如果一个数组中的元素是另一个数组，就构成了包含数组的数组，即二维数组。
		+	二维数组和一维数组的声明方式一样，只是将数组中的一个或多个元素也声明成一个数组，同时也有直接为数组元素赋值和使用 array() 函数两种声明二维数组的方法。
		+	数组的不同维度标志着我们需要使用几个下标（索引）来获取对应的数组元素，比如二维数组需要使用两个下标来获取对应的数组元素，三维数组则需要三个，以此类推。
	```
	$arr1[0]['name'] = '张三';
	$arr1[0]['chinese'] = 89;
	$arr1[0]['math']  = 80;
	$arr1[0]['english'] = 90;
	$arr1[1]['name'] = 'lily';
	$arr1[1]['chinese'] = 92;
	$arr1[1]['math'] = 78;
	$arr1[1]['english'] = 93;
	var_dump($arr1);
	$arr2 = array(
	    0 => array('name' => 'simons', 'chinese' => 77, 'math' => 82, 'english' => 90),
	    1 => array('name' => 'shiden', 'chinese' => 92, 'math' => 94, 'english' => 98)
	);
	var_dump($arr2);
	```
	- 	获取数组长度
		+	count()，统计出数组里所有的元素的数量，或对象的属性个数。如果 $array 既不是数组，也不是对象，count() 函数将返回 1；如果 $array 等于 NULL，则 count() 函数返回 0。
		```
		count(array $arr [, $mode = COUNT_NORMAL]);
		参数说明如下：
		$array：为待统计的数组或对象；
		$mode：为可选参数，可以省略。
		如果省略 $mode 参数，或者将其设置为 COUNT_NORMAL 或者 0，count() 函数将不检测多维数组；
		如果 $mode 设置为 COUNT_RECURSIVE 或者 1，count() 函数将递归计算数组中元素的个数，对于计算多维数组的元素个数尤为有用。
		```
		+	sizeof()，sizeof() 函数是 count() 函数的别名，也就是所 sizeof() 函数的功能及使用方法与 count() 函数完全相同。
	- 	数组排序函数
		+	sort()，对数组元素进行升序排序（从小到大）。改变原数组键值
		+	rsort()，对数组元素进行降序排序（从大到小）。
		+	asort()，对数组元素进行升序排序（从小到大）。并保持索引关系
		+	arsort()，对数组元素进行降序排序（从大到小）。并保持索引关系
		+	ksort()，按照数组的键值对数组进行升序排序（从小到大）。并保持索引关系
		+	krsort()，按照数组的键值对数组进行降序排序（从大到小）。并保持索引关系
		```
		sort(array $arr [, $sort_flags = SORT_REGULAR]);
		sort() 函数会删除 $array 中原有的键名并为其赋与新的键名，而不是仅仅将数组元素重新排序。函数执行成功时会返回 TRUE，失败时会返回 FALSE。

		$array：为要排序的数组。
		$sort_flags：为可选参数，默认为“SORT_REGULAR”，用来定义函数排序的模式。$sort_flags 可以设置为下面这些值：
		0 或 SORT_REGULAR：正常比较数组元素，不改变其类型（默认值）；
		1 或 SORT_NUMERIC：将数组元素当作数字来比较；
		2 或 SORT_STRING：将数组元素当作字符串来比较
		3 或 SORT_LOCALE_STRING：根据当前的区域（locale）设置来把数组元素当作字符串比较，可以用 setlocale() 来改变。
		4 或 SORT_NATURAL：和 natsort() 类似对每个数组元素以“自然的顺序”对字符串进行排序，是 PHP5.4.0 中新增的。
		5 或 SORT_FLAG_CASE：能够与 SORT_STRING 或 SORT_NATURAL 合并（OR 位运算），不区分大小写排序字符串。
		```
		```
		$sortArr = array(
		    'a' => 5,
		    'c' => 9,
		    'n' => 23,
		    's' => 34,
		    't' => 89
		);
		$numArr = array(78, 3, 56, 34, 9, 107);
		$websiteArr = array(
		    'name'   => 'My Own Websit',
		    'url'    => 'http://test.com/mytest.php',
		    'title'  => 'Welcome to my little site.',
		    'system' => 'linux'
		);

		sort($numArr);
		var_dump($numArr);
		// sort($sortArr);
		arsort($sortArr);
		rsort($sortArr);
		// ksort($sortArr);
		var_dump($sortArr);

		// sort();rsort();
		// asort();arsort();
		// ksort($websiteArr);
		arsort($websiteArr);
		var_dump($websiteArr);
		```
	- 	返回数组当前的元素
		+	在 PHP 中，每个数组都有一个内部的指针指向它“当前的”单元（元素），这个指针最初指向的是当前数组中的第一个单元。
		+	current(array $arr)，可以获取内部指针指向元素的值，current() 函数可以返回当前内部指针指向的数组元素的值，但它并不会移动指针，如果需要移动指针的话需要与其它函数配合使用；如果内部指针指向超出了数组的末端，current() 函数会返回 FALSE。
		+	pos()，可以将 pos() 函数当作是 current() 函数的别名。
	- 	向上或向下移动数组指针
		+	next()，将内部指针指向数组中的下一个元素；
		+	prev()，将内部指针指向数组中的上一个元素；
	- 	其他数组指针操作
		+	end()，将内部指针指向数组中的最后一个元素；
		+	reset()，将内部指针指向数组中的第一个元素；
		+	，在使用 foreach 语句对数组遍历操作时，它会自动把指针指向数组第一个元素，然后才开始遍历，所以就不必使用 reset() 函数。如果我们使用 each() 函数遍历数组，就需要使用 reset() 将数组指针复位到起始位置。
	- 	返回数组中当前元素的键名
		+	key()，用来获取数组中当前元素的键名`mixed key(array $arr);`
	- 	返回数组当前元素的键值对
		+	注意：自 PHP7.2 起，each() 函数已经被弃用。
		+	each()，返回当前元素的键名和键值作为一个数组，并将内部指针指向数组中的下一个元素。`array each(array $arr);`
		+	可用while循环和each遍历数组
		```
		while(list($key, $val) = each($arr)) {
			echo $key.' => '.$val.PHP_EOL;
		}
		```
	- 	判断数组的键名或索引是否存在
		+	array_key_exists()，可以判断给定的键名是否存在与数组中。在一个数组中键名是唯一的，所以不需要对其数据类型进行判断。
	```
	bool array_key_exists(mixed $key, array $arr);
	$key 为要检查的键名；$array 为一个数组，array_key_exists() 函数可以检查数组 $array 中是否存在 $key 这个键名，存在时函数返回 TRUE，不存在时返回 FALSE。
	array_key_exists() 函数仅能判断数组第一维的键名，不能判断多维数组里嵌套的键名。
	```
	- 	判断数组中是否存在某个值
		+	in_array()，可以查找数组中是否包含某个值。如果存在则返回true，否则返回false
		+	in_array() 函数的第一个参数除了可以是一个具体的值外，还可以是一个数组。也就是说使用 in_array() 函数还可以判断一个数组是否包含另一个数组。不区分值的先后顺序
		```
		bool in_array(mixed $needle, array $arr [, $strict = FALSE]);
		$needle：为待搜索的值，如果 $needle 是字符串，则在比较时区分大小写；
		$array：为待搜索的数组；
		$strict：为可选参数，默认为 FALSE。
		如果 $strict 为空或者 FALSE，则 in_array() 函数只会检查 $needle 的值是否和 $array 中的值相等；
		如果 $strict 的值为 TRUE，in_array() 函数除了会检查 $needle 和 $array 中的值之外，还会比较它们的类型是否相等。
		in_array() 函数只适用于在一维数组中查找某个元素，不会递归查找数组中每个维度的元素。
		```
	- 	foreach()遍历数组
		+	 foreach，遍历数组时与下标无关。不管是不是连续的索引数组还是以字符串为下标的关联数组。foreach 只能应用于数组，自 PHP5 起，还可以遍历对象。
		```
		foreach(array $arr as $value) {
			echo $value;
		}
		foreach(array $arr as $key => $value) {
			echo $key.' => '.$value;
		}
		```
	- 	数组转字符串
		+	implode()，可以将一个数组转化为字符串
		```
		string implode([string $glue, ] array $arr);
		$glue 用来设置一个字符串，表示使用 $glue 将数组每个元素连接在一起，默认情况下 $glue 为空字符串；$array 为需要转换的数组。
		implode() 函数的 $glue 参数是可选的，可以省略。
		```
	- 	将数组中的值赋值给一组变量
		+	list()，可以把数组中的值分别赋给一组变量。像 array() 一样，它并不是真正的函数，而是语言结构。 list() 可以在单次操作内为一组（多个）变量赋值
		```
		list($var1 [, $val2 ...]) = array $arr;
		其中 $val1、$val2 是待赋值的一组变量，多个变量之间使用逗号,分隔。
		list() 中的变量是按照数组的索引顺序赋值的，并且索引要从 0 开始，list() 仅能用于索引数组，并且索引要从 0 开始。PHP5 里 list() 从最右边的参数开始赋值；而 PHP7 里 list() 从最左边的参数开始赋值。
		```
	- 	数组的键/值操作
		+	array_keys()，获取数组中的所有元素的键名
		+	array_values()，获取数组中的所有元素的键值
		+	array_slip()，交换数组中的键/值
		+	array_key_exists()，检测键名是否存在数组中
		+	array_search()，在数组中检索给定值并返回键名或索引
	- 	数组的拆分和合并
		+ 	array_merge()，合并数组	
		+	array_slice()，截取数组的一部分
		+	array_chunk()，分割数组
	- 	数组的填充和清除
		+	array_splice()，删除数组中的一部分并用其他值代替
		+	array_pad()，用给定的值填充数组
		+	array_push()，在数组尾部添加元素
		+	array_pop()，删除数组尾部的元素
		+	array_shift()，删除数组开头的元素
		+	array_unshift()，在数组开头插入元素
		+	array_fill()，以填充数据的方式创建新数组
		+	array_fill_keys()，使用指定的键和值来填充数组
	- 	数组的计算
		+	array_sum()，计算数组中所有元素的和
		+	array_product()，计算数组中所有元素的乘积
	- 	其他数组函数
		+	is_array()，判断是否为一个数组
		+	array_rand()，随机获取数组元素
		+	shuffle()，随机打乱数组
*	如果将一个对象转换成对象，它将不会有任何变化。如果其它任何类型的值被转换成对象，将会创建一个内置类 stdClass 的实例。如果该值为 NULL，则新的实例为空。 array 转换成 object 将使键名成为属性名并具有相对应的值，除了数字键，不迭代就无法被访问。
*	资源类型resource，
	-	资源 resource 是一种特殊变量，保存了外部资源的一个引用，如打开文件、数据库连接等，资源是通过专门的函数来建立和使用的。
	-	由于资源类型变量保存有为打开文件、数据库连接、图形画布区域等的特殊句柄，因此将其它类型的值转换为资源没有意义。
*	NULL，
	-	特殊的 NULL 值表示一个变量没有值。NULL 类型唯一可能的值就是 NULL。被赋值为 NULL；尚未被赋值；被 unset()。
	-	使用 (unset) 将一个变量转换为 null 将不会删除该变量或 unset 其值。仅是返回 NULL 值而已
*	预定义变量
	-	$GLOBALS — 引用全局作用域中可用的全部变量
	-	$_SERVER —  服务器和执行环境信息
	-	$_ENV  —  环境变量
	-	$_REQUEST  —  HTTP Request变量
	-	$_GET  — HTTP GET 变量
	-	$_POST  —  HTTP POST变量
	-	$_FILES  —  HTTP文件上传变量
	-	$_SESSION  —  Session变量
	-	$_COOKIES  —  HTTP cookies
	-	$HTTP_RAW_POST_DATA   —  原生的post数据
	-	$http_response_header  —  http的响应头
	-	$php_errormsg  — 前一个错误信息
*	以下预定义变量只在命令行中使用
	-	$argc  —  传递给脚本的参数数目
	-	$argv  —  传递给脚本的参数数组

### 变量及其作用域
*	全局作用域
*	局部变量，全局变量
- 	全局变量。函数内部是不允许使用在函数外部定义的全局变量的，但是有些时候却需要在函数内使用这些全局变量，那么我们该怎么办呢？使用 PHP 中的 global关键字就可以让我们在函数内部使用在函数外部定义的全局变量，global 关键字后面可以跟多个变量作为参数，多个变量之间以“,`global $a, $b;`
- 	global 关键字，只能在函数内部使用，不能在函数外部使用；
- 	global 关键字只能用来引用函数外部的全局变量，在引用时不能直接赋值，赋值和声明语句需要分开写；
- 	在函数内部销毁一个使用 global 关键字修饰的变量时，函数外部的变量不受影响。
- 	与 global 关键字功能类似的还有 $GLOBALS, $GLOBALS 是一个预定义的超全局数组，其中包含了全局作用域中的所有可用变量，变量的名字就是数组的键。
- 	与 global 相比，$GLOBALS 有一下几点不同：global $var 指的是对函数外部同名变量的引用，是两个互不影响的变量，而 $GLOBALS['var'] 指的是函数外部变量本身，是一个变量。$GLOBALS 不限定必须在函数内部使用，在程序的任意位置都能使用。
*	静态变量（static）（与JS闭包机制差不多）
- 	静态变量在初始化之后，会在程序运行期间会一直存在
- 	和局部变量相比，静态变量具有一下特点：当函数执行完毕后，静态变量不会消失；静态变量只能在函数内部使用；静态变量只会被初始化一次； 静态变量初始化的值可以省略，默认值为 null；静态变量的初始值只能是具体的字符串、数值等，而不能是一个表达式。注意：在函数外面使用静态变量时并不会报错，这时它的生命周期与作用域和全局变量是一样的；在函数内部定义静态量时，它的生命周期也和全局变量一样，但是作用域和局部变量的作用域一样的。
- 	静态变量虽然在程序的整个执行过程中始终存在，但是它的作用域和局部变量是一样的，在作用域之外是不能使用的。
```
<?php

function test()
{
    static $a = 0;
    echo $a.PHP_EOL;
    $a++;
}

test();
test();
```
*	可变变量
- 	可变变量还可用于数组，但是要解决一个摸棱两可的问题。如：$$a[1]，要使用{}来明确想要$a[1]作为一个变量，还是$$a作为一个变量并取出其中索引为[1]的值。`${$a[1]};{$$a}[1]`
```
<?php

$a = 'hello';
$$a = 'world';

var_dump($a, $hello);

$a = 'b';
$b = 'c';
$c = 'd';

$$$$a = 'bcd';

var_dump($d);
```

### 常量
*	常量定义
	-	命名方式
		*	通常常量用大写字母表示，并且遵循和变量一样的命名规范，即以字母或下划线开头，后面跟任何字母，数字或下划线。
		*	避免使用 __ 两个下划线开头，被预留为 PHP 内置魔术常量使用。
	-	常量定义，一个常量一旦被定义，就不能再改变或者取消定义。常量就是不能改变的量，PHP 中常量一旦被定义，就不能被修改或取消定义。
		*	常量前面没有美元符号（$）；常量的作用域是全局的；常量只能用 define() 和 const 定义；常量一旦被定义就不能被重新定义或者取消定义。
		*	使用define()方法
		```
		<?php

		define('HELLO', 'Hello');
		//  通常使用defined() 来判断一个常量是否被定义
		definded('SHIYANLOU') or define('SHIYANLOU', 'shiyanlou');
		```
		*	const关键字定义类之外的常量
		```
		<?php

		// 注意使用 const 只能在类外部定义，且必须处于最顶端的作用区域，
		// 因为用此方法是在编译时定义的。这就意味着不能在函数内，循环内
		// 以及 if 语句之内用 const 来定义常量。
		const HELLO = 'Hello';
		const SHIYANLOU = 'shiyanlou';

		class Test
		{
		    function sayHi()
		    {
		        define('HELLO', 'Hi');

		        echo HELLO;
		    }
		}

		$t = new Test();
		$t->sayHi();
		```
		*	获取常量的值
			- 	除了可以直接使用常量名外，还可以使用 constant() 函数，使用函数和直接使用常量名的效果是一样的。但使用函数可以动态输出不同的常量，在使用上要灵活、方便得多，
			```
			define('DEFAULT_PATH', '/www/website/example');
			const WEBSITE = 'http://mytest.com/index.php';
			$defaultPath = 'DEFAULT_PATH';
			$website = 'WEBSITE';
			echo constant('DEFAULT_PATH');
			echo constant($website);
			```
*	与变量比较
	-	相同点，命名规范都必须以字母或下划线开头，后面跟字母，数字或下划线
	-	不同点
		*	常量前面没有美元符号 $
		*	常量只能通过 define() 或 const 定义，而不能通过赋值语句
		*	常量可以不用理会变量的作用域而在任何地方定义和访问
		*	常量一旦定义就不能被重新定义或者取消定义
		*	常量的值只能是标量
*	魔术常量(预定义常量)
	-	PHP 向它运行的任何脚本提供了大量的预定义常量。不过很多常量都是由不同的扩展库定义的，只有在加载了这些扩展库时才会出现，或者动态加载后，或者在编译时已经包括进去了。
	- 	魔术常量通常以两个下划线__开头，并以两个下划线__结尾。
	-	有八个魔术常量它们的值随着它们在代码中的位置改变而改变。这些特殊的常量不区分大小写。
		*	__LINE__，文件的当前行号。
		*	__FILE__，文件的完整路径和文件名，如果用在被包含在文件中，则返回被包含的文件名。
		*	__DIR__，文件所在的目录。如果用在被包含的文件中，则返回被包含的文件的所在目录。
		*	__FUNCTION__，函数名称，返回该函数被定义时的名字。
		*	__CLASS__，类的名称，返回该类被定义时的名字。
		*	__METHOD__，类的方法名，返回该方法被定义时的名字（区分大小写）。
		*	__NAMESPACE__，当前命名空间的名称（区分大小写）。
		*	__TRAIT__，Trait的名字。自 PHP 5.4 起此常量返回 trait 被定义时的名字（区分大小写）。Trait 名包括其被声明的作用区域（例如 Foo\Bar）。

### 运算符
*	数学运算符
	-	-$a，取反
	-	$a + $b，加法，a 和a和b 的和
	-	$a - $b，减法，a 和a和b 的差
	-	$a * $b，乘法，a 和a和b 的积
	-	$a / $b，除法，a 和a和b 的商
	-	$a % $b，取余，a除以b 的余数
	-	$a ** $b，乘方，a 的a的b 次方
*	赋值运算符（传值赋值和引用赋值）
*	位运算符（转换为二进制进行比较，返回十进制结果）
```
$a & $b，And（按位与），将把 a 和a和b 中都为 1 的位设为 1。
$a | $b，Or（按位或），将把 a 和a和b 中任何一个为 1 的位设为 1。
$a ^ $b，Xor（按位异或），将把 a 和a和b 中一个为 1 另一个为 0 的位设为 1。
~$a，Not（按位取反），将 $a 中为 0 的位设为 1，反之亦然。
$a << $b，Shift left（左移），将 a 中的位向左移动a中的位向左移动b 次（每一次移动都表示乘以 2）。
$a >> $b，Shift right（右移），将 a 中的位向右移动a中的位向右移动b 次（每一次移动都表示除以 2）。
```
*	比较运算符
```
$a == $b，如果类型转换后 a 等于a等于b，返回 TRUE。
$a === $b，如果 a 等于a等于b，并且它们的类型也相同，返回 TRUE。
$a != $b，如果类型转换后 a 不等于a不等于b，返回 TRUE。
$a <> $b，等同于 !=
$a !== $b，如果 a 和a和b 的值或类型不同，返回 TRUE。
$a < $b ，如果 a 严格小于a严格小于b，返回 TRUE。
$a > $b，如果 a 严格大于a严格大于b，返回 TRUE。
$a <= $b，如果 a 小于或者等于a小于或者等于b，返回 TRUE。
$a >= $b，如果 a 大于或者等于a大于或者等于b，返回 TRUE。
如果比较一个数字和字符串或者比较涉及到数字内容的字符串，则字符串会被转换为数值并且比较按照数值来进行。
此规则也适用于 switch 语句。当用 === 或 !== 进行比较时则不进行类型转换，因为此时类型和数值都要比对。
```
```
<?php

var_dump(null == "");
var_dump(null == false);
var_dump(true > false);
var_dump(0 == "a");
var_dump("1" == "01");
var_dump("10" == "1e1");
var_dump(100 == "1e2");
var_dump([4,5] < [1,2,3]);
var_dump((object)"Test" > "Test");
var_dump((object)"Test" > [2,3]);

switch ("a") {
case 0:
    echo "0";
    break;
case "a":
    echo "a";
    break;
}

从结果可以看出

null 或 String 和 string 比较时，将 null 转换为 ""，进行数字或词汇比较
bool 或 null 和其他类型比较时，转换为 bool，FALSE < TRUE
string，resource 或 number 相互比较时，将字符串或资源转换为数字，按普通数字比较
array 之间比较时，具有较少成员的数组较小
object 和其他类型比较时，object 总是更大
array 和其他类型比较时，array 总是更大，但是比对象小
switch 中第一个条件满足时，不会执行后面满足条件的语句
```
*	错误控制运算符
	-	PHP 支持一个错误控制运算符：\@。当将其放置在一个 PHP 表达式之前，该表达式可能产生的任何错误信息都被忽略掉。
	-	误控制运算符只对表达式有效。对新手来说一个简单的规则就是：如果能从某处得到值，就能在它前面加上 \@ 运算符。例如，可以把它放在变量，函数和 include 调用，常量，等等之前。不能把它放在函数或类的定义之前，也不能用于条件结构例如 if 和 foreach 等。
	```
	<?php

	$my_file = @file ('non_existent_file') or die ("Failed opening file: error was '$php_errormsg'");

	$value = @$cache[$key];
	```
*	执行运算符
	-	PHP 支持一个执行运算符：反引号（\`\`）。注意这不是单引号！PHP 将尝试将反引号中的内容作为外壳命令来执行，并将其输出信息返回（例如，可以赋给一个变量而不是简单地丢弃到标准输出）。
	```
	<?php
	$output = `ls -al`;
	echo "<pre>$output</pre>";
	注意，反引号运算符在激活了安全模式或者关闭了 shell_exec() 时是无效的。
	```
*	定界符(\<\<\<)
	- 	因为 PHP 是一个 Web 编程语言，在编程过程中难免会遇到使用 echo 来输出大段的 HTML 代码或者 Javascript 脚本的情况。如果用传统字符串输出的话，肯定要使用大量的转义字符来对字符串中的特殊字符进行转义，比如单引号''、双引号""等等，难免会出现语法错误。
	- 	PHP 中的定界符能够定义一段较长的字符串，并且可以按照原样输出在其内部的东西，包括换行、缩进等格式，在定界符中的任何特殊字符都不需要转义，而且定界符中的变量也能被解析。这也是为什么 PHP 要引入定界符的原因之一。
	- 	定界符标识必须前后一致；
	- 	可以任意定义定界符标识，比如 echo、html、div，尽量选用有意义的标识符，并遵循命名规范；
	- 	起始标识符后不能跟任何字符，空格也不可以，另起一行后再输入文本内容；
	- 	结束标识符后面要紧跟一个分号，并且前后都不能有任何字符，即结束标识要顶头写，且独占一行，其后除紧跟分号外，不能有任何字符（空格也不可以）；
	- 	最后要注意的是，结束标识所在行不能成为脚本的最后一行，其下必须有空行或者其他代码行，否则报错。
	```
	$partStr = <<<str
		some long sentences;
str;
	```
*	递增（减）运算符
```
++$a，a 的值加一返回a的值加一返回a。
$a++，返回 a，然后将a，然后将a 的值加一。
--$a，a 的值减一返回a的值减一返回a。
$a--，返回 a，然后将a，然后将a 的值减一。

// 递增（减）运算符对布尔和 NULL 类型的影响
<?php

$a = null;
$b = true;

var_dump(++$a, --$a, ++$b, --$b);
// 布尔值不受影响，NULL 递增为 1，递减为 0
```
*	逻辑运算符
```
$a and $b，逻辑与，如果 a 和a和b 都为 TRUE
$a && $b，逻辑与，如果 a 和a和b 都为 TRUE，其中 && 优先级高于 and
$a or $b，逻辑或，如果 a 或a或b 任一为 TRUE
$a || $b，逻辑或，如果 a 或a或b 任一为 TRUE，|| 优先级高于 or
$a xor $b，逻辑异或，如果 a 或a或b 任一为 TRUE，但不同时是，则返回 TRUE
! $a，逻辑非，如果 $a 不为 TRUE
&&，|| 的优先级高于 =，= 的优先级高于 and，or。
```
*	字符串运算符
	-	有两个字符串运算符。第一个是连接运算符 . ，它返回其左右参数连接后的字符串，第二个是连接赋值运算符 .= ，它将右边参数附加到左边的参数后。
*	数组运算符
```
$a + $b，a 和a和b 的联合
$a == $b，a 和a和b 键和值都相同则为 TRUE
$a === $b，a 和a和b 键和值且顺序和类型都相同返回 TRUE
$a != $b，a 和a和b 中键或值不同返回 TRUE
$a <> $b，等同于 !=
$a !== $b，a 和a和b 中键，值，顺序或类型，其中一个不相同则返回 TRUE
```
*	类型运算符
*	php7新增操作符
	-	组合比较符
		*	太空船操作符使用 <=> 表示，用于比较两个表达式。当 a 小于、等于或大于a小于、等于或大于b 时它分别返回-1、0 或 1。 比较的原则是沿用 PHP 的常规比较规则进行的。
		```
		<?php
		// 整数
		echo 1 <=> 1; // 0
		echo 1 <=> 2; // -1
		echo 2 <=> 1; // 1

		// 浮点数
		echo 1.5 <=> 1.5; // 0
		echo 1.5 <=> 2.5; // -1
		echo 2.5 <=> 1.5; // 1

		// 字符串
		echo "a" <=> "a"; // 0
		echo "a" <=> "b"; // -1
		echo "b" <=> "a"; // 1
		?>
		```
	-	NULL 合并运算符
		*	NULL 合并运算符使用 ?? 表示，意味着如果 ?? 之前的变量存在且值不为 NULL，它就会返回自身的值，否则返回 ?? 后的操作数。
		*	合并运算符通常可用三元运算符作为替换，多个合并运算符的优先级从左到右一次执行。
		```
		<?php

		$username = $_GET['user'] ?? 'nobody';
		$username = isset($_GET['user']) ? $_GET['user'] : 'nobody';

		$username = $_GET['user'] ?? $_POST['user'] ?? 'nobody';
		```
*	运算符优先级

### 控制结构
*	if/else语句
```
if () 
	echo "...";
if () {
	echo "...";
}
if ():
	echo "...";
endif;

if () {
	echo "...";
}
else{
	echo "...";
}

if () {
	echo "...";
}
else if () {
	echo "...";
}
else{
	echo "...";
}

// 注意: 必须要注意的是 elseif 与 else if 只有在类似上例中使用花括号的情况下才认为是完全相同。
// 如果用冒号来定义 if/elseif 条件，那就不能用两个单词的 else if，否则 PHP 会产生解析错误。
<?php

/* 不正确的使用方法： */
if($a > $b):
    echo $a." is greater than ".$b;
else if($a == $b): // 将无法编译
    echo "The above line causes a parse error.";
endif;


/* 正确的使用方法： */
if($a > $b):
    echo $a." is greater than ".$b;
elseif($a == $b): // 注意使用了一个单词的 elseif
    echo $a." equals ".$b;
else:
    echo $a." is neither greater than or equal to ".$b;
endif;
```
*	流程控制的替代语法
	-	PHP 提供了一些流程控制的替代语法，包括 if，while，for，foreach 和 switch。替代语法的基本形式是把左花括号（{）换成冒号（:），把右花括号（}）分别换成 endif;，endwhile;，endfor;，endforeach; 以及 endswitch;
*	while语句
	-	while 语句的含意很简单，它告诉 PHP 只要 while 表达式的值为 TRUE 就重复执行嵌套中的循环语句。表达式的值在每次开始循环时检查，所以即使这个值在循环语句中改变了，语句也不会停止执行，直到本次循环结束。有时候如果 while 表达式的值一开始就是 FALSE，则循环语句一次都不会执行。
	```
	while(Expr) {
		{statement}
	}
	while (true):
		// todo something
	endwhile;
	```
	-	do while语句
	```
	do {
		// to do something
	} while (true);
	```
*	for语句
```
for (expr1; expr2; expr3) {
	// to do something
}
for (expr1; expr2; expr3):
	// to do something
endfor;

for ($i = 0; $i < 10; $i++) {
	echo $i;
	$i++;
}
for ($i = 0;;$i++) {
	if ($i > 10) {
		break;
	}
	echo $i;
}
$i = 0
for (;;) {
	if ($i > 10) {
		break;
	}
	echo $i;
	$i++
}
if ($i = 0, $j = 0; $i < 10; $j += $i, print $i, $i++);

$people = Array(
	Array("name" => "Kalle", "salt" => 856412),
	Array("name" => "Pierre", "salt" => 215863),
);

for ($i = 0; $length = sizeof($people), $i < $length; $i++) {
	$people[$i]['salt'] = rand(000000, 999999);
}
```
*	foreach语句
	-	foreach 语法结构提供了遍历数组的简单方式。foreach 仅能够应用于数组和对象，如果尝试应用于其他数据类型的变量，或者未初始化的变量将发出错误信息。
	```
	foreach($arr as $value) {
		// to do something
	}
	foreach($arr as $key => $value) {
		// to do something
	}
	// 第一种格式遍历给定的 array 数组。每次循环中，当前单元的值被赋给 $value 并且数组内部的指针向前移一步。
	// 第二种格式做同样的事，只除了当前单元的键名也会在每次循环中被赋给变量 $key。
	```
	-	改变foreach数组中元素的值
	```
	// 在 $value 之前加上 &来修改数组的元素。
	// 通过 $key 重新赋值。
	$arr1 = $arr2 = [1, 2, 3, 4];
	foreach($arr as &$value) {
		$value = $value *2;
	}
	var_dump($arr1, $value);
	foreach($arr2 as $key => $value) {
		$arr2[$key] = $value * 2;
	}
	var_dump($arr2, $value);
	// 从结果可以看出，数组最后一个元素的 $value 引用在 foreach 循环之后仍会保留。建议使用 unset() 来将其销毁。
	```
	- 	在 PHP 7 中，按照值进行循环时，foreach 是对数组的复制操作，在循环过程中对数组的修改不会影响循环行为，但在 PHP 5 中却会有影响。
	```
	$arr = [0, 1, 2];
	foreach($arr as $val) {
		var_dump($val);
		unset($arr[1]);
	}
	// php7中的输出为
	int(0) int(1) int(2)
	// php5中的输出为
	int(0) int(2)
	```
	- 	在 PHP 7 中按照引用循环的时候对数组的修改会影响循环，在 PHP 5 中 则不会改变。
	```
	$arr = [0];
	foreach($arr as &$val) {
		var_dump($val);
		$arr[1] = 1;
		$arr[2] = 2;
	}
	// php7中的输出为
	int(0) int(1) int(2)
	// php5中的输出为
	int(0)
	```
*	break语句
	-	break可用于for, foreach, while, do/while或者switch结构的执行。break 可以接受一个可选的数字参数来决定跳出几重循环（由里向外数）。
*	continue语句
	-	continue 在循环结构用来跳过本次循环中剩余的代码并在条件求值为真时开始执行下一次循环。
	-	Note: 注意在 PHP 中 switch 语句被认为是可以使用 continue 的一种循环结构。
	-	continue 接受一个可选的数字参数来决定跳过几重循环到循环结尾。默认值是 1，即跳到当前循环末尾。
	```
	$i = 0;
	while($i++ < 3) {
		echo "Outer".PHP_EOL;
		while(1) {
			echo "Middle".PHP_EOL;
			while(1) {
				echo "Inner".PHP_EOL;
				continue 3;
				// break 3;
			}
			echo "There is never get output.".PHP_EOL;
		}
		echo "Neither does here.".PHP_EOL;
	}
	```
*	switch语句
	-	当if/else中由多个if if/elseif条件的时候，可以用switch语句来替换.
	-	switch 中每个条件需要一对 case 和 break。
	-	default 匹配任何条件。如果匹配了某个条件，但是该语句中没有使用 break，则 default 中的语句将会被执行。
	-	注意和其它语言不同，continue 语句作用到 switch 上的作用类似于 break。如果在循环中有一个 switch 并希望 continue 到外层循环中的下一轮循环，用 continue 2。
	-	在 **switch 语句中条件只求值一次**并用来和每个 case 语句比较。在 **elseif 语句中条件会再次求值**。如果条件比一个简单的比较要复杂得多或者在一个很多次的循环中，那么用 switch 语句可能会快一些。
	-	case 表达式可以是任何求值为简单类型的表达式，即整型或浮点数以及字符串。不能用数组或对象，除非它们被解除引用成为简单类型。
	```
	if($a == "apple") {
		// do something
	}
	elseif($a == "banana") {
		// do something
	}
	elseif($a == "orange") {
		// do something
	}
	else{
		// do something
	}

	switch ($a) {
		case "apple":
			// do something ;
			break;
		case "banana":
			// do something
			break;
		case "orange":
			// do something ;
			break;
		default:
			// do something;
	}
	// switch 支持替代语法的流程控制。
	switch($a):
		case "apple":
			// do something ;
			break;
		case "banana":
			// do something ;
			break;
		case "orange":
			// do something ;
			break;
		default:
			// do something ;
			break;
	endswitch;

	// 在一个 case 中的语句也可以为空，这样只不过将控制转移到了下一个 case 中的语句
	<?php
	switch ($i) {
	    case 0:
	    case 1:
	    case 2:
	        echo "i is less than 3 but not negative";
	        break;
	    case 3:
	        echo "i is 3";
	}
	?>
	// 一个 case 的特例是 default。它匹配了任何和其它 case 都不匹配的情况。例如：
	<?php
	switch ($i) {
	    case 0:
	        echo "i equals 0";
	        break;
	    case 1:
	        echo "i equals 1";
	        break;
	    case 2:
	        echo "i equals 2";
	        break;
	    default:
	        echo "i is not equal to 0, 1 or 2";
	}
	?>
	```
*	declare语句
	-	declare 结构用来设定一段代码的执行指令。
	```
	<?php
	declare (directive);
		statement
	```
	-	directive 部分允许设定 declare 代码段的行为。目前只认识两个指令：ticks以及 encoding。
	-	declare 代码段中的 statement 部分将被执行——怎样执行以及执行中有什么副作用出现取决于 directive 中设定的指令。
	-	declare 结构也可用于全局范围，影响到其后的所有代码（但如果有 declare 结构的文件被其它文件包含，则对包含它的父文件不起作用）
	```
	<?php
	// these are the same:

	// you can use this:
	declare(ticks=1) {
	    // entire script here 
	    //	statement
	}

	// or you can use this:
	declare(ticks=1);
	// entire script here
	//	statement
	?>
	```
	-	directive（指令）,tick和encoding
		*	tick
			+	Tick（时钟周期）是一个在 declare 代码段中解释器每执行 N 条可计时的低级语句就会发生的事件。N 的值是在 declare 中的 directive 部分用 ticks=N 来指定的。
			+	不是所有语句都可计时。通常条件表达式和参数表达式都不可计时。
			+	在每个 tick 中出现的事件是由 register_tick_function() 来指定的。更多细节见下面的例子。注意每个 tick 中可以出现多个事件。
			```
			declare (ticks=1);

			function tick_handler() {
				echo "tick_handler() called\n";
			}

			register_tick_function("tick_handler");

			$a = 1;

			if($a < 3) {
				$a += 2;
				print($a);
			}

			declare (ticks=1);

			function tick_handler() {
				echo "tick_handler() called\n";
			}

			$a = 1;
			tick_handler();
			if($a < 3) {
				$a += 2;
				tick_handler();
				print($a);
				tick_handler();
			}
			tick_handler();
			```
		*	encoding
			+	可以使用encoding指令来对每段脚本指定其编码方式。
			```
			declare(encoding='ISO-8859-1');
			//	code here
			```
*	return语句
	-	如果在一个函数中调用 return 语句，将立即结束此函数的执行并将它的参数作为函数的值返回。
	```
	function sayHello() {
		return "hello";
		echo "Hello";
	}
	echo sayHello();
	```
	-	return 可以不接任何参数。
	```
	function sayNothing() {
		return ;
	}
	sayNothing;
	```
*	包含语句
	- 	文件包含是指将另一个源文件的全部内容包含到当前源文件中进行使用，通常也称为引入外部文件。引用外部文件可以减少代码的重用性
	-	include ''
	-	include_once ''
	-	require ''
	-	require_once ''
	-	区别和比较
		+	include 和 require 都可以加载文件，不同点在于如果加载的文件包含错误，include 发出警告，继续执行后面的语句，而 require 则发出致命错误，终止程序执行。
		+	include_once 和 require_once 的区别同 include 和 require，都是遇到错误时是否继续执行。
		+	include 和 include_once，以及 require 和 require_once 的区别在于是否进行重复检测。
		+	include_once 语句和 include 语句类似，唯一的区别就是如果包含的文件已经被包含过，就不会再次包含。include_once 可以确保在脚本执行期间同一个文件只被包含一次，以避免函数重定义、变量重新赋值等问题。
		+	require_once 语句是 require 语句的延伸，它的功能与 require 语句基本类似，不同的是，在应用 require_once 语句时会先检查要包含的文件是不是已经在该程序中的其他地方被包含过，如果有，则不会再次重复包含该文件。
		```
		<?php
		include 'a.php';
		require 'app/bbb.php';
		```
	-	文件包含
		+	参数是绝对路径（以 / 开头的路径），则直接包含该文件;
		+	参数是相对路径或文件名，按照 include_path （可以通过 phpinfo() 查看当前包含的路径）指定的目录寻找。
		+	如果在 include_path 下没找到该文件则在调用脚本文件所在的目录和当前工作目录下寻找。
		+	如果最后仍未找到文件则 include 结构会发出一条警告；这一点和 require 不同，后者会发出一个致命错误。
	-	变量范围
		+	当一个文件被包含时，其中所包含的代码继承了 include 所在行的变量范围。从该处开始，调用文件在该行处可用的任何变量在被调用的文件中也都可用。不过所有在包含文件中定义的函数和类都具有全局作用域。

### 函数
*	函数特点
	- 	函数是唯一的：每个函数都有唯一的名称，在程序的其他部分使用该名称，可以执行函数中的语句，称为调用函数。
	- 	函数是独立的：无须程序其他部分的干预，函数便能够单独执行其任务。
	- 	函数能执行特定的任务：任务是程序运行时所执行的具体工作，如将一行文本输出到浏览器、对数组进行排序、计算立方根等。
	- 	函数可以将一个返回值返回给调用它的程序：程序调用函数时，将执行该函数中的语句，而这些语句可以将信息返回给调用它们的程序。
*	使用函数的好处
	- 	提高程序的重用性；
	- 	提高软件的可维护性；
	- 	提高软件的开发效率；
	- 	提高软件的可靠性；
	- 	控制程序设计的复杂性。
*	预定义函数
*	用户自定义函数
	-	函数定义
	```
	function foo(arg1, arg2, ..., argn) {
		// do something
		return $retval;
	}
	```
	-	命名规范：函数名和 PHP 中的其它标识符命名规则相同。有效的函数名以字母或下划线打头，后面跟字母，数字或下划线。
	- 	不管是自定义的函数还是系统函数，如果函数不被调用，就不会执行。只要在需要使用函数的位置，使用函数名称和参数列表进行调用即可。
	-	函数无需在调用前被定义，除非函数是**有条件被定义或者在函数中调用函数**，一般都无须在调用函数之前定义。
	```
	function actionA() {
		echo "A";
	}
	actionA();// 可以调用
	actionB();// 可以调用
	function actionB() {
		echo "B";
	}

	// 函数中调用函数
	function actionA() {
		function actionB() {
			echo "B";
		}
	}
	actionB(); // 无法调用
	actionA(); // 可以调用，定义函数actionB()
	actionB(); // 可以调用
	```
	-	PHP 不支持函数重载，也不可能取消定义或者重定义已声明的函数。在 PHP 中不能使用函数重载，所以不能定义重名的函数，也包括不能和系统函数同名；
	```
	function sayHi() {
		echo "Hi";
	}
	function sayHi(){
		echo "hi";
	}
	sayHi(); //  报错，不能重新定义函数 sayHi();
	```
	-	递归函数
		+	递归函数的本质是函数调用函数本身，但是要**避免递归函数／方法**，调用超过 100-200 层，因为可能会使堆栈崩溃从而使当前脚本终止。 无限递归可视为编程错误。
		+	递归函数即自调用函数，也就是函数在函数体内部直接或间接地自己调用自己。需要注意的是使用递归函数时通常会在函数体中附加一个判断条件，以判断是否需要继续执行递归调用，当条件满足时会终止函数的递归调用。
		+	递归函数最大的好处在于可以精简程序中繁杂重复的程序，并且能以这种特性来执行一些较为复杂的运算动作。例如列表、动态树型菜单以及遍历目录等操作。相应的非递归函数虽然效率高，但却比较难编程，而且相对来说可读性差。
		+	递归思想。递归的主要思想就是把一个相对复杂的问题（原始问题）转化为一个个与原问题相似的规模较小的问题（子问题）来解决，等一个个小问题解决了，最终的大问题自然就解决了。
		+	递归方法只需少量的程序就可描述出解题过程所需要的多次重复计算，大大减少程序的代码量。当然，递归函数也不是完美的，也有一定的缺点，那就是递归方法函数的运行效率不高。
		+	要实现递归，需满足以下两个条件：子问题需与原始问题为同样的事，且更为简单。不能无限制地调用本身，必须有一个出口，化简为非递归状况处理。
		```
		function recursion($a) {
			if($a < 29) {
				echo "$a\n";
				recursion($a + 1);
			}
		}
		```
		```
		在 PHP 中最大递归层数也不是没有限制的，这与程序的内存限额有关，PHP5 默认允许一个程序使用 128M 的内存，
		因此当递归层数过大导致 128M 内存耗尽时，程序就会产生一个致命错误并退出。PHP7 默认允许使用 256M 的内存。
		PHP 允许使用的最大内存可以通过修改 php.ini 文件来修改，如下所示：
		; Maximum amount of memory a script may consume (128MB)
		; http://php.net/memory-limit
		memory_limit=256M
		```
*	函数的参数
	-	通过参数列表可以传递信息到函数，即以逗号作为分隔符的表达式列表。
	-	PHP 支持按值传递参数（默认），通过引用传递参数以及默认参数。也支持可变数量的参数；
	-	缺省情况下，函数参数通过值传递（因而即使在函数内部改变参数的值，它并不会改变函数外部的值）。如果希望允许函数修改它的参数值，必须通过引用传递参数。 如果想要函数的一个参数总是通过引用传递，可以在函数定义中该参数的前面预先加上符号 &
	- 	在 PHP 5 中已引入函数的参数类型声明，如果给定的值不是一个合法的参数类型，那么在 PHP 5 中会出现一个 Fatal error，在 PHP 7 中则会抛出一个 TypeError exception。
	- 	PHP 7 中增加了参数可声明的类型种类
| 				类型	 			    | 				说明 				| 	php版本 	|
|				---					| 				---				|		---		|
| class, interface 				| 参数必须是指定类或接口的实例| 5.0	|
| Array								| 参数为数组类型			| 5.1				|
| Callable							| 参数为有效的回调类型| 5.4				|
| Bool								| 参数为布尔值				| 7.0				|
| Float								| 参数为浮点型				| 7.0				|
| Int									| 参数为整型					| 7.0				|
| String 							| 参数为字符串类型		| 7.0				|
	- 形参的类型声明，及其基础类型的自动转换
	```
	<?php
	function actionB(&$string)
	{
	    $string .= 'and something extra.';
	}
	$str = 'This is a string, ';
	actionB($str);
	echo $str;    // outputs 'This is a string, and something extra.'

	// 函数形参类型指定为类的情况
	class A{}
	class B extends A{}
	class C {}

	function f(A $cls) {
	    echo get_class($cls).PHP_EOL;
	}

	$a = new A();
	$b = new B();
	$c = new C();
	f($a);
	f($b);
	f($c);

	// 函数形参的自动类型转换
	function foo(string $a, int $b, string $c) {
	    echo '$c + $b = '.($c+$b).PHP_EOL;
	    // echo '$b + $c = '.($b+$c).PHP_EOL;
	    echo "the string is $a".PHP_EOL;
	}

	foo('hello', 3.2, 1);
	?>
	```
	- 	严格模式
		*	在 PHP 7 中，可以使用declare(strict_types=1)设置严格模式，这样只有在传递的参数与函数期望得到的参数类型一致时才能正确执行，否则会抛出错误。只有一种情况例外，就是当函数期望得到的是一个浮点型数据而提供的是整型时，函数也能正常被调用。
	```
	<?php
	declare(strict_types=1);

	function foo(int $a, string $b, string $c) {
		echo '$c + $b = '.($c+$b).PHP_EOL;
	    echo "the string is $a".PHP_EOL;
	}
	foo('hello', 3.2, 1);
	```
	-	设置参数的默认值
	```
	<?php
	function makecoffee($type = "cappuccino")
	{
	    return "Making a cup of $type.\n";
	}
	echo makecoffee();
	echo makecoffee(null);
	echo makecoffee("espresso");
	?>
	```
	- 	在调用函数时，需要向函数传递参数，被传入函数的参数称为实参，而函数定义的参数称为形参。而向函数传递参数的方式有四种，分别是值传递、引用传递、默认参数和可变长度参数。
		*	值传递。值传递是 PHP 中函数的默认传值方式，也称为“拷贝传值”。顾名思义值传递的方式会将实参的值复制一份再传递给函数的形参，所以在函数中操作参数的值并不会对函数外的实参造成影响。因此如果不希望函数修改实参的值，就可以通过值传递的方式。
		*	引用传递。参数的引用传递就是把实参的内存地址复制一份，然后传递给函数的形参，实参和形参都指向同一个内存地址，因此函数对形参的操作，会影响到函数外的实参。按引用传递就是将实参的内存地址传递到函数的形参中。因此实参和形参指向的是同一个内存地址。这时在函数内部的所有操作都会影响到函数外实参的值。引用传递的方式就是在值传递的基础上加上一个&符号，
		*	默认参数。默认参数就是给函数的某个或多个形式参数指定一个默认的值，如果调用函数时不传入对应的值，那么函数就会使用这个默认值，这样可以避免调用时出现没有参数的错误，也可以使一些程序显得更加合理。如果传入对应的参数，就会替换这个默认值。默认参数也可以是多个，而且默认参数必须放在非默认参数右边，并且指定默认参数的值必须是一个具体的值，如数字、字符串，而不能是一个变量。
		*	可变长度参数。函数的形式参数可使用…来表示函数可接受一个可变数量的参数，可变参数将会被当作一个数组传递给函数。
		```
		function foo(...$arr) {
			var_dump($arr);
		}
		foo(1, 2, 3);
		foo(1, 2, 3, 4, 5);
		```
*	返回值
	-	值通过使用可选的返回语句（return）返回。可以返回包括数组和对象的任意类型。返回语句会立即中止函数的运行，并且将控制权交回调用该函数的代码行。
	```
	<?php
	function square($num)
	{
	    return $num * $num;
	}
	echo square(4);   // outputs '16'.
	```
	-	函数不能返回多个值，但可以通过返回一个数组来得到类似的效果。
	```
	function small_number() {
		return array(1, 2, 3);
	}
	list($a, $b, $c) = small_number();
	```
	-	从函数返回一个引用，必须在函数声明和指派返回值给一个变量时都使用以用操作符&
	```
	function &return_reference() {
		return $someref;
	}
	$newref =& return_reference();
	```
	- 	return 语句用于向“调用函数者”返回一个值，返回值后，立即结束函数运行，所以 return 语句一般都放在函数的末尾；
	- 	如果一个函数中存在多个 return 语句，则只会执行第 1 个；
	- 	return 语句也可以不返回参数，就相当于结束函数运行；
	- 	如果在全局作用域内使用 return 语句，则会立即终止当前运行的脚本；
	- 	如果使用 include 或 require 引入的脚本文件中含有 return 语句，则会返回到引入脚本的地方继续向下执行，return 之后的其它代码不再执行。
	-	返回值类型声明
		+	PHP 7 增加了对返回类型声明的支持。 类似于参数类型声明，返回类型声明指明了函数返回值的类型。可用的类型与参数声明中可用的类型相同。
		```
		// 声明了传入可变长度的形参类型为array并且声明了返回值的类型为array
		function arraysSum(array ...$arrays): array {
			return array_map(function (array $array): int {
				return array_sum($array);
			}, $arrays);
		}
		print_r(arraysSum([1, 2, 3], [ 4, 5, 6], [ 7, 8, 9]));
		```
*	可变函数
	-	PHP 支持可变函数的概念。这意味着如果一个变量名后有圆括号，PHP 将寻找与变量的值同名的函数，并且尝试执行它。可变函数可以用来实现包括回调函数，函数表在内的一些用途。
	-	变量函数不能用于语言结构，例如 echo， print， unset()， isset()， empty()， include， require 以及类似的语句。需要使用自己的包装函数来将这些结构用作变量函数。
	```
	class TestA{
		public static $actionB = "property B\newt_resize_screen()";

		public function actionA() {
			echo "Method A\n";
		}

		public static function actionB() {
			echo "Methon B\n";
		}
	}

	function sayHi() {
		echo "Hi\n";
	}

	function sayHello($word = '') {
		echo "Hello $word.\n";
	}

	$hi = "sayHi";
	$hi();

	$hello = "sayHello";
	$hello("world");

	$a = "actionA";
	(new TestA())->$a();

	echo TestA::$actionB;
	$actionB = "actionB";
	TestA::$actionB();
	// 可以用可变函数的语法来调用一个对象方法和静态方法。
	// 静态方法调用优先级高于属性调用
	```
*	内部函数（内置函数）
	-	PHP 有很多标准的函数和结构。还有一些函数需要和特定地 PHP 扩展模块一起编译，否则在使用它们的时候就会得到一个致命的未定义函数错误。例如，要使用 image 函数中的 imagecreatetruecolor()，需要在编译 PHP 的时候加上 GD 的支持。或者，要使用 mysql_connect() 函数，就需要在编译 PHP 的时候加上 MySQL 支持
	-	调用 phpinfo() 或者 get_loaded_extensions() 可以得知 PHP 加载了那些扩展库。同时还应该注意，很多扩展库默认就是有效的。PHP 手册按照不同的扩展库组织了它们的文档。
	-	Note: 如果传递给函数的参数类型与实际的类型不一致，例如将一个 array 传递给一个 string 类型的变量，那么函数的返回值是不确定的。在这种情况下，通常函数会返回 NULL。但这仅仅是一个惯例，并不一定如此。
	-	参见 function_exists()，函数参考，get_extension_funcs() 和 dl()。
*	匿名函数（Anonymous functions）
	-	匿名函数（Anonymous functions），也叫闭包函数（closures），允许 临时创建一个没有指定名称的函数。最经常用作回调函数（callback）参数的值。当然，也有其它应用的情况。
	-	匿名函数目前是通过 Closure 类来实现的
	```
	<?php
	echo preg_replace_callback('~-([a-z])~', function ($match) {
	    return strtoupper($match[1]);
	}, 'hello-world');
	// 输出 helloWorld
	```
	-	闭包函数也可以作为变量的值来使用。PHP 会自动把此种表达式转换成内置类 Closure 的对象实例。把一个 closure 对象赋值给一个变量的方式与普通变量赋值的语法是一样的，最后也要加上分号
	-	匿名函数的变量赋值
	```
	$greet = function ($name) {
		printf("Hello %s\r\n", $name);
	};
	$greet("world");
	$greet("PHP");
	```
	- 	use关键字
		*	使用use关键字，闭包函数可以实现从父作用域中继承变量，但是从php7.1开始，不支持继承预定义变量和$this。
		*	需要注意的是，匿名函数虽然可以继承父级作用域中的变量，但是在匿名函数中修改变量的值不会对父级作用域中的变量造成影响。如果想要在修改匿名函数继承的变量的同时，同样修改其父级作用域中的变量，则需要在变量名的前面添加 & 符号，类似于函数中的引用传递。
	-	从父作用域继承变量
	```
	
	$msg = "hello";

	$a = function() {
		var_dump($msg);
	};
	$a();

	$b = function() use($msg) {
		var_dump($msg);
	};
	$b();

	$msg = "Hi";
	$b();	

	$c = function() use(&$msg) {
		var_dump($msg);
	};
	$c();

	$d = function($arg) use($msg) {
		var_dump($arg.'  '.$msg);
	};
	$d("Hello");

	// 使用 use 可以从父作用域继承变量
	// 匿名函数是在定义的时候继承父作用域变量，而不是在调用的时候继承
	// 变量使用引用赋值，则原变量发生改变，则引用该变量的变量也会发生变化
	```
*	回调函数
	- 	回调函数，就是指调用函数时并不是向函数中传递一个标准的变量作为参数，而是将另一个函数作为参数传递到调用的函数中，这个作为参数的函数就是回调函数。通俗的来说，回调函数也是一个我们定义的函数，但是不是我们直接来调用的，而是通过另一个函数来调用的，这个函数通过接收回调函数的名字和参数来实现对它的调用。
	- 	PHP 还提供了两个内置函数 call_user_func() 和 call_user_func_array() 来对回调函数进行支持。这两个函数的区别是 call_user_func_array() 是以数组的形式接收回调函数的参数，而 call_user_func() 则是以具体的参数来接收回调函数参数的。
	- 	call_user_func()，会把第一个参数作为回调函数来调用，`call_user_func($callback [, parameter, ..., ])`，第一个参数$callback是被调用的回调函数，其余参数是回调函数的参数，多个参数之间使用,分隔。
	- 	call_user_func_array()，可以调用回调函数，并使用一个数组来作为回调函数的参数。`call_user_func_array($callback, $param_arr)`，第一个参数$callback是被调用的回调函数，$param_arr是一个索引数组，用来存储需要传入回调函数中的具体参数。
	```
	function arithmetic($funcName, $m, $n) {
	    return $funcName($m, $n);
	}
	function add($m, $n) {
	    return $m + $n;
	}

	function arithmetic($funcName, $m, $n) {
	    return call_user_func($funcName, $m, $n);;
	}
	function add($m, $n) {
	    return $m+$n;
	}

	function arithmetic($funcName, $m, $n) {
	    return call_user_func_array($funcName, array($m, $n));
	}
	function add($m, $n) {
	    return $m+$n;
	}
	echo arithmetic('add', 7, 4);
	```


### 类与对象
*	类
	- 	面向对象编程
		+	面向对象编程（Object Oriented Programming，OOP）是一种编程思想，在很多现代编程语言中都有面向对象编程的概念。
		+	面向对象编程的思想就是把具有相似特性的事物抽象成类，通过对类的属性和方法的定义实现代码共用。将实现某一特定功能的代码部分进行封装，这样可被多处调用，而且封装的粒度越细小被重用的概率越大。
		+	而面向对象编程的继承性和多态性也提高了代码的重用率。总之，面向对象编程充分地体现了软件编程中的“高内聚，低耦合”的思想。
	-	类的定义
		+	每个类的定义都以关键字 class 开头，后面跟着类名（非保留字）。
		+	类名后跟着一对花括号，里面包含有类属性和方法的定义。
	-	类成员默认值
		+	在定义类属性的时候，可以使用默认值
	-	对象与类（创建实例）
		+	要创建一个对象的实例，使用关键字 new。`$a = new classA();`
	-	对象赋值
		+	当把一个对象已经创建的实例赋给一个新变量时，新变量会访问同一个实例，就和用该对象赋值一样。此行为和给函数传递入实例时一样。可以用克隆给一个已创建的对象建立一个新实例。
	-	伪变量$this（当前对象）
		+	伪变量 $this 可以在当一个方法在对象内部调用时使用。$this 是一个到调用对象（通常是方法所属于的对象，但也可以是另一个对象，如果该方法是从第二个对象内静态调用的话）的引用。
		+	 PHP 面向对象编程中，对象一旦被创建，在对象中的每个成员方法里面都会存在一个特殊的对象引用“$this”。成员方法属于哪个对象，“$this”就代表哪个对象，与连接符->联合使用，专门用来完成对象内部成员之间的访问。
		+	在使用 $this 访问某个成员属性时，后面只需要跟属性的名称即可，不需要$符号。另外，$this 只能在对象中使用，其它地方不能使用 $this，而且不属于对象的东西 $this 也调用不了，可以说没有对象就没有 $this。
*	对象继承
	-	一个类可以在声明中用 extends 关键字继承另一个类的方法和成员。PHP 不支持多重继承，一个类只能继承一个类。
	-	被继承的方法和成员可以通过用同样的名字重新声明被覆盖，除非父类定义方法时使用了 final 关键字。可以通过 parent:: 来访问被覆盖的方法或成员。
	- 	子类可以增加父类之外的新功能，因此也可以将子类称为父类的“扩展”。此外，子类还可以继承父类的构造函数，当子类被实例化时，PHP 会先在子类中查找构造函数。如果子类有自己的构造函数，PHP 会先调用子类中的构造函数。当子类中没有时，PHP 则会去调用父类中的构造函数。
	- 	在 C++ 中，一个子类可以继承一个基类，也可以继承多个基类。继承一个基类称为单继承；继承多个基类称为多继承。但在 PHP 中没有多继承，只能使用单继承模式。也就是说，一个类只能直接从另一个类中继承数据。但一个类可以有多个子类。
	- 	类成员的访问控制
		+	父类中使用public修饰的成员均可被子类继承
		+	很多情况下有些类继承的属性是不想在类外部被访问的，这时就可以把这个成员声明为一个保护成员。使用protected来修饰该成员。受保护的成员不可在类的外部访问，但是可以在子类的内部访问，也就是说我们可以在子类设置一个成员函数来访问这个受保护成员。
		+	类中使用pravite修饰的成员被称为私有变量，父类中的私有成员不会被子类继承，因此不能被子类访问到。
*	命名空间
	- 	什么是命名空间？从广义上来说，命名空间是一种封装事物的方法，在很多地方都可以见到这种抽象概念。例如，在操作系统中目录用来将相关文件分组，对于目录中的文件来说，它就扮演了命名空间的角色。举个简单的例子，文件 foo.txt 可以同时在目录 /home/greg 和 /home/other 中存在，但在同一个目录中不能存在两个 foo.txt 文件。另外，在目录 /home/greg 外访问 foo.txt 文件时，我们必须将目录名以及目录分隔符放在文件名之前，例如 /home/greg/foo.txt。这个原理应用到程序设计领域就是命名空间的概念。
	- 	PHP 命名空间可以解决以下两类问题：用户编写的代码与 PHP 内部的类/函数/常量或第三方类/函数/常量之间的命名冲突；为很长的标识符名称（通常是为了缓解第一类问题而定义的）创建一个别名（或简短）的名称，以提高源代码的可读性。
	- 	虽然任意合法的 PHP 代码都可以包含在命名空间中，但只有类（包括抽象类和 traits）、接口、函数和常量等类型的代码受命名空间的影响。
	- 	在声明命名空间之前除了用于定义源文件编码方式的 declare 语句外，所有非 PHP 代码（包括空白符）都不能出现在命名空间声明之前。另外，与 PHP 其它的语言特征不同，同一个命名空间可以定义在多个文件中，即允许将同一个命名空间的内容分割存放在不同的文件中。
	- 	定义命名空间
	```
	namespace Foo;
	//  与目录和文件的关系很象，PHP 中的命名空间也允许指定层次化的命名空间名称。因此，命名空间的名字可以使用分层次的方式定义：
	namespace Directory\Subdirectory\Foo;
	namespace MyProject\Controller\Home;

	namespace Foo\Bar\Side {
		...
	}
	```
	- 	使用命名空间
		+	PHP 命名空间中的元素使用同样的原理。例如，命名空间下的类名可以通过三种方式引用：
		+	非限定名称，或不包含前缀的类名称，例如$a=new foo();或foo::staticmethod();。如果当前命名空间是 currentnamespace，那么 foo 将被解析为 currentnamespace\foo。如果使用 foo 的代码是全局的，不包含在任何命名空间中的代码，则 foo 会被解析为 foo。
		+	限定名称，或包含前缀的名称，例如$a = new subnamespace\foo();或subnamespace\foo::staticmethod();。如果当前的命名空间是 currentnamespace，则 foo 会被解析为 currentnamespace\subnamespace\foo。如果使用 foo 的代码是全局的，不包含在任何命名空间中的代码，foo 会被解析为 subnamespace\foo。
		+	完全限定名称，或包含了全局前缀操作符的名称，例如$a = new \currentnamespace\foo();或\currentnamespace\foo::staticmethod();。在这种情况下，foo 总是被解析为代码中的文字名 currentnamespace\foo。
		+	如果命名空间中的函数或常量未定义，则该非限定的函数名称或常量名称会被解析为全局函数名称或常量名称。
		+	访问任意全局类、函数或常量，都可以使用完全限定名称，例如 \strlen() 或 \Exception 等。
		+	PHP 允许通过别名引用或导入的方式来使用外部的命名空间，这是命名空间的一个重要特征。
		+	使用 use 关键字可以实现命名空间的导入，从 PHP5.6 开始允许导入函数和常量，并为它们设置别名。
		+	为了简化操作，PHP 还支持在一行中导入多个命名空间，中间使用,隔开，示例代码如下：
		```
		use Foo;
		use Foo\Bar\Demo as Demo;
		use My\Full\ClassName as Another, My\Full\NSname;

		// 导入操作是编译执行的，但动态的类名称、函数名称或常量名称则不是。
		// 实例化一个 My\Full\ClassName对象
		$anotherObj = new Another();
		$a = 'Another';
		// 实例化一个Another对象
		$obj = new $a();
		```
	- 	namespace关键字和__NAMESPACE__常量
		+	PHP 支持两种抽象的访问当前命名空间内部元素的方法，__NAMESPACE__ 魔术常量和 namespace 关键字。
		+	__NAMESPACE__ 常量的值是包含当前命名空间名称的字符串。在全局的，不包括在任何命名空间中的代码，它是一个空的字符串。
		+	namespace 关键字可用来显式访问当前命名空间或子命名空间中的元素，它等价于类中的 self 操作符。
		```
		<?php
	    namespace MyProject;
	    use Foo\Bar\Demo as demo;       // 导入 Foo\Bar\Demo 命名空间
	    blah\mine();                    // 调用 MyProject\blah\mine() 函数
	    namespace\blah\mine();          // 调用 MyProject\blah\mine() 函数
	    namespace\func();               // 调用 MyProject\func() 函数
	    namespace\sub\func();           // 调用 MyProject\sub\func() 函数
	    namespace\cname::method();      // 调用 MyProject\cname 类的静态方法 method
	    $a = new namespace\sub\cname(); // 实例化 MyProject\sub\cname 对象
	    $b = namespace\CONSTANT;        // 将常量 MyProject\CONSTANT 的值赋给 $b
		```
	- 	命名空间名称解析规则
		+	在说明名称解析规则之前，我们先来看看命名空间名称的定义：
		+	非限定名称：名称中不包含命名空间分隔符的标识符，例如 Foo；
		+	限定名称：名称中含有命名空间分隔符的标识符，例如 Foo\Bar；
		+	完全限定名称：名称中包含命名空间分隔符，并以命名空间分隔符开始的标识符，例如 \Foo\Bar。namespace\Foo 也是一个完全限定名称。
		+	对完全限定名称的函数，类和常量的调用在编译时解析。例如 new \A\B 解析为类 A\B；
		+	所有的非限定名称和限定名称（非完全限定名称）根据当前的导入规则在编译时进行转换。例如，如果命名空间 A\B\C 被导入为 C，那么对 C\D\e() 的调用就会被转换为 A\B\C\D\e()；
		+	在命名空间内部，所有的没有根据导入规则转换的限定名称均会在其前面加上当前的命名空间名称。例如，在命名空间 A\B 内部调用 C\D\e()，则 C\D\e() 会被转换为 A\B\C\D\e()；
		+	非限定类名根据当前的导入规则在编译时转换（用全名代替短的导入名称）。例如，如果命名空间 A\B\C 导入为 C，则 new C() 被转换为 new A\B\C()；
		+	在命名空间内部（例如 A\B），对非限定名称的函数调用是在运行时解析的。例如对函数 foo() 的调用是这样解析的：
		+	在当前命名空间中查找名为 A\B\foo() 的函数；
		+	尝试查找并调用全局空间中的 foo() 函数。
		+	在命名空间（例如 A\B）内部对非限定名称或限定名称类（非完全限定名称）的调用是在运行时解析的。下面是调用 new C() 及 new D\E() 的解析过程：
		+	new C() 的解析：
		+	在当前命名空间中查找 A\B\C 类；
		+	尝试自动装载类 A\B\C。
		+	new D\E() 的解析：
		+	在类名称前面加上当前命名空间名称变成：A\B\D\E，然后查找该类；
		+	尝试自动装载类 A\B\D\E。
		+	为了引用全局命名空间中的全局类，必须使用完全限定名称 new \C()。
*	属性（类成员）
	-	类的变量成员叫做属性。属性声明是由访问控制关键字 public，protected 或 private 和一个变量来组成，同时可以加上默认值。
	-	访问属性，在类的成员方法里面，可以通过 $this-> 加变量名来访问类的属性和方法，但是要访问类的静态属性或者在静态方法要使用 self:: 加变量名。注意 self:: 这种方式后的变量名需要加 $ 符号，而 $this-> 后的变量名不需要加
*	常量
	-	在类中可以定义常量。常量的值将始终保持不变。在定义和使用常量的时候不用使用$符号。
	-	在接口（interface）中也可以定义常量。
	```
	class A{
		const ENV = 'env';
		const HELLO = 'Hello';
	}
	interface B{
		const ENV = 'env';
		public function sayHi() {}
	}
	```
*	自动加载对象
	-	要进行一个类操作时，需要先将该类加载进来，例如 include，require 等。
	-	如果要执行的类很多，则需要大量 include 操作，会导致重复加载，管理苦难等一系列问题。在 PHP 5 中，不用这样做了，可以使用 spl_autoload_register() 函数来注册任意数量的自动加载器
*	构造函数和析构函数
	-	构造函数`void __constuct()`
		+	构造函数（constructor method，也称为构造器）是类中的一种特殊函数，当使用 new 关键字实例化一个对象时，构造函数将会自动调用。
		+	构造函数就是当对象被创建时，类中被自动调用的第一个函数，并且一个类中只能存在一个构造函数。和普通函数类似构造函数也可以带有参数，如果构造函数有参数的话，那么在实例化也需要传入对应的参数，
		+	如果没有在代码中显式地声明构造函数，类中会默认存在一个没有参数列表并且内容为空的构造函数。如果显式地声明构造函数则类中的默认构造方法将不会存在。所以构造函数通常用来做一些准备工作，比如为某些参数赋值等。
		+	如果显式地声明构造函数，那么它的访问权限必须是 public，而且构造函数是在实例化时自动调用的，我们不需要手动调用。
		+	析构函数的声明格式与构造函数相似，在类中声明析构函数的名称也是固定的，同样以两个下画线开头的方法名__destruct()，而且析构函数不能带有任何参数。
		+	在 PHP 中析构函数并不是很常用，它属于类中可选的一部分，只有需要的时候才在类中声明。
	-	创建一个对象时（ new 操作），构造函数会自动调用，实例化 A 的时候执行构造函数。
	-	析构函数`void __destruct(void)`
		+	析构函数的作用和构造函数正好相反，析构函数只有在对象被垃圾收集器收集前（即对象从内存中删除之前）才会被自动调用。析构函数允许我们在销毁一个对象之前执行一些特定的操作，例如关闭文件、释放结果集等。
		+	在 PHP 中有一种垃圾回收机制，当对象不能被访问时就会自动启动垃圾回收机制，收回对象占用的内存空间。而析构函数正是在垃圾回收机制回收对象之前调用的。
	-	析构函数会在到某个对象的所有引用都被删除或者当对象被显式销毁时执行。
	-	注意: 如果子类中定义了构造函数则不会隐式调用其父类的构造函数。要执行父类的构造函数，需要在子类的构造函数中调用 parent::__construct()。和构造函数一样，父类的析构函数不会被引擎暗中调用。要执行父类的析构函数，必须在子类的析构函数体中显式调用 parent::__destruct()。
	-	析构函数即使在使用 exit() 终止脚本运行时也会被调用。在析构函数中 调用 exit() 将会中止其余关闭操作的运行。
*	访问控制
	-	对属性或方法的访问控制，是通过在前面添加关键字 public、protected 或 private 来实现的。如果未添加，则默认为 public。
	-	public 所定义的类成员可以在任何地方被访问
	-	protected 所定义的类成员则可以被其所在类的子类和父类访问（当然，该成员所在的类也可以访问）
	-	private 定义的类成员则只能被其所在类访问
*	范围解析操作符
	-	范围解析操作符，可以简单地说是一对冒号，可以用于访问静态成员、方法和常量，还可以用于覆盖类中的成员和方法。
*	static关键字
	-	声明类成员或方法为 static，就可以不实例化类而直接访问。不能通过一个对象来访问其中的静态成员（静态方法除外）。
	-	由于静态方法不需要通过对象即可调用，所以伪变量 $this 在静态方法中不可用。静态属性不可以由对象通过 -> 操作符来访问。
	-	注意：在 PHP7 中通过（::）调用非静态方法会产生一个 E_DEPRECATED 级别的警告，不赞成这样使用，在以后可能会取消对这种用法的支持。
	-	就像其它所有的 PHP 静态变量一样，静态属性只能被初始化为一个字符值或一个常量，不能使用表达式。 所以你可以把静态属性初始化为整型或数组，但不能指向另一个变量或函数返回值，也不能指向一个对象。
*	抽象类
	- 	在面向对象语言中，一个类可以有一个或多个子类，而每个类都应该至少有一个公有方法作为外部代码访问它的入口。而抽象类和抽象方法是在 PHP5 中引入的一个概念，主要是为了方便类继承。
	-	定义为抽象的类可能无法直接被实例化，任何一个类， 如果它里面至少有一个方法是被声明为抽象的，那么这个类就必须被声明为抽象的。如果类方法被声明为抽象的， 那么其中就不能包括具体的功能实现。
	-	继承一个抽象类的时候，子类必须定义父类中的所有抽象方法。
	-	这些方法的访问控制必须和父类中一样（或者更为宽松）。
	-	此外方法的调用方式必须匹配，即类型和所需参数数量必须一致。
	- 	抽象方法
		+	抽象方法是没有方法体的方法，所谓的没有方法体指的就是，在声明方法时候没有花括号{ }以及其中的内容，而是直接在方法名后加上分号结束。另外，在声明抽象方法时要使用“abstract”关键字修饰。
		```
		// abstract 访问权限修饰符 function 方法名1(参数列表);
		abstract public function abstractFuncDemo($argument1, $argument2...);
		```
	- 	抽象类
		+	只要一个类里面有一个方法是抽象方法，那么这个类就必须定义为抽象类，抽象类也必须要用'abstract'关键字来修饰，抽象类中也可以包含不是抽象方法的成员方法和成员属性，但访问权限不能是私有的(private)，因为抽象类中的方法需要被子类继承。
		+	抽象类就像是一个“半成品”的类，在抽象类中包含没有被实现的抽象方法，所以抽象类是不能被实例化的，即创建不了对象，也就不能直接使用它。既然抽象类是一个“半成品”的类，那么使用抽象类有什么作用呢？
		+	可以将抽象类看作是为它的子类定义公共接口，将它的操作（可能是部分也可能是全部）交给子类去实现。就是将抽象类作为子类重载的模板使用的，定义抽象类就相当于定义了一种规范，这种规范要求子类去遵守。
		+	当子类继承抽象类以后，就必须把抽象类中的抽象方法按照子类自己的需要去实现。子类必须把父类中的抽象方法全部都实现，否则子类中还存在抽象方法，所以还是抽象类，也就不能实例化为对象。
		+	另外需要注意的是，在子类中成员方法的访问权限可以和抽象方法的访问权限相同，但不能更加严格。而且，子类中成员方法的参数个数应该和抽象方法的参数个数一样。
	```
	abstract class AbstractClassDemo {
		abstract function demoFunc($arg1, $arg2);
		public function testFunc() {}
	}
	```
*	接口
	- 	继承的特性简化了对象、类的创建，增加了代码的重用性。但 PHP 只支持单继承，也就是说每个类只能继承一个父类。为了解决这个问题，PHP 引入了接口。接口是一种特殊的抽象类，而抽象类又是一种特殊的类，所以可以将接口看作是一种特殊的类。
	-	使用接口（interface），你可以指定某个类必须实现哪些方法，但不需要定义这些方法的具体内容。
	-	我们可以通过 interface 来定义一个接口，就像定义一个标准的类一样，但其中定义所有的方法都是空的。 
	-	接口中定义的所有方法都必须是 public，这是接口的特性。
	-	接口实现。要实现一个接口，可以使用 implements 操作符。类中必须实现接口中定义的所有方法，否则会报一个 fatal 错误。如果要实现多个接口，可以用逗号来分隔多个接口的名称。
	-	实现多个接口时，接口中的方法不能有重名。
	-	接口也可以继承，通过使用 extends 操作符。
	-	接口中也可以定义常量。接口常量和类常量的使用完全相同。 它们都是定值，不能被子类或子接口修改。
	- 	接口的声明
		+	如果抽象类中的所有方法都是抽象方法，我们就可以使用另外一种声明方式——“接口”技术。我们都知道类的声明是使用“class”关键字，而接口的声明则是使用“interface”关键字。
		+	接口中所有的方法都是抽象方法，而且不需要在方法前使用 abstract 关键字进行修饰。而且在接口中也不需要显示地使用 public 访问权限来进行修饰，因为默认权限就是 public 的，也只能是 public（公有的）。另外，接口中不能声明变量，只能使用 const 关键字声明为常量类型的成员属性。
		+	接口和抽象类一样也不能实例化为对象，它是一种更严格的规范，也需要通过子类来实现。与抽象类不同的是，接口可以直接使用接口名称在接口外面获取常量成员的值。
		+	因为接口不能进行实例化操作，所以要使用接口中的成员，就必须借助子类。在子类中继承接口需要使用 implements 关键字，如果要实现多个接口的继承，那么每个接口之间使用逗号,分隔。
		+	在使用 implements 关键字继承接口的同时，还可以使用 extends 关键字来继承一个类。也就是说，可以在继承一个类的同时实现多个接口，但一定要先使用 extends 继承类再去使用 implements 实现多个接口。
		+	既然要通过子类继承了接口中的方法，那么接口中的所有方法都必须在子类中实现，否则 PHP 将抛出如下所示的错误信息：
		+	我们还可以使用 extends 关键字让一个接口去继承另一个接口，实现接口之间的扩展。
		+	如果需要使用抽象类去实现接口中的部分方法，也需要使用 implements 关键字。
		+	说了这么多，那么使用接口具体有什么作用呢？其实它的作用很简单，当有很多人一起开发一个项目时，每个人都可能会去调用别人写的一些类。这时有人就会问了，我怎么知道别人的某个功能的实现方法是怎么命名的呢？这个时候 PHP 接口就起到作用了。
		+	简单来说，我们可以将接口看作一个类的模板或者类的规定。如果你属于这类，你就必须遵循这个类的规定，少一个都不行，但是具体怎么去做，那是你的事。也就是说我们可以定义一些接口，每个接口中都包含若干的抽象方法。在多人开发时，每个人都根据自己的需要来实现一部分接口，这样就可以避免我们在调用别人开发的方法时不知道方法名的尴尬了。
		```
		interface Demo {
			// 类常量成员
			const NAME = 'testName';
			const URL = 'http://test.com';
			// 抽象方法
			function printUrl();
			function funcDemo();
		}
		class TestClass implements Demo {
			public function printUrl() {
				echo self::URL;
			}
			public function funcDemo(){
				echo 'Hello world.';
			}
		}

		class 类名 extends 父类名 implements 接口一, 接口二, ..., 接口 n {
		    // 实现所有接口中的抽象方法
		}
		```
	```
	<?php

	interface A
	{
	    public function actionA();
	}

	interface B
	{
	    public function actionB();
	}

	//实现多个接口
	class C implements A, B
	{
	    public function actionA()
	    {
	        //do something
	    }
	    public function actionB()
	    {
	        //do something
	    }
	}
	```
*	final，最终类和最终方法
	- 	final关键字，被修饰的类不可继承，被修饰的方法不可以在子类中重写。不可以用于类成员属性
	- 	在 PHP5 中新增加了 final 关键字，它可以在类或类中成员方法前使用，但是不能用来修饰成员属性。final 的中文含义是最终的、最后的，所以使用 final 修饰的类可以称为“最终类”，使用 final 修饰的成员方法可以称为“最终方法”
	- 	如果某个类不想被继承，那么就可以使用 final 来修饰这个类。使用 final 修饰的类不能有子类，也就不能对它进行拓展。如果想要继承一个使用 final 修饰的类，程序将会报错
	- 	如果类中的某个方法已经很完善了，不需要再被重写，那么就可以使用 final 来修饰这个方法。如果在子类中试图重写这个使用 final 修饰的方法，程序同样会出现错误，
	- 	final 关键字应该放在其它修饰词（例如 public、static 等）之前使用。
	```
	final class ClassDemo {
		public function foo() {
			...
		}
		final public function demoFunc() {
			...
		}
	}
	```
*	clone关键字
	- 	PHP 中的对象模型是通过引用来调用对象的，但有时需要建立一个对象的副本，在改变原有对象时不希望影响到对象副本。如果使用new关键字重新创建对象，再为属性赋上相同的值，这样做会比较烦琐而且也容易出错。在 PHP 中可以根据现有的对象克隆出一个完全一样的对象，克隆以后，原本对象和副本对象是完全独立互不干扰的。
	- 	因为 clone 的方式实际上是对整个对象的内存区域进行了一次复制并用新的对象变量指向新的内存，因此赋值后的对象和原对象之间是相互独立的。
	- 	对象克隆成功后，它们中的成员方法、属性以及值是完全相同的。如果要对克隆后副本的成员属性重新赋值，
	- 	如果使用=将一个对象赋值给一个变量，那么这时得到的将是一个对象的引用，通过这个变量更改属性的值将会影响原来的对象。
	- 	\_\_clone魔术方法
		+	 \_\_clone魔术方法不能够被直接调用，只有当通过clone关键字克隆一个对象时才可以使用该对象调用clone方法。当创建对象的副本时，php会检查__clone()对象是否存在。如果不存在则调用默认的__clone()方法，复制对象的所有属性。如果__clone()方法已经定义过，那么__clone()方法就会负责设置新对象的属性。所以在__clone()方法中只要覆盖那些需要更改的属性就可以了。
		+	\_\_clone()方法不需要任何参数。
		+	如果在类中设置一个空的，访问权限为 private（私有的）的 \_\_clone() 方法的话，可以起到禁止克隆的作用。（单例设计模式中使用）
	```
	$obj = new ClassDemo();
	$newObjCopy = clone $obj;
	```
*	instanceof 判断对象是否属于某个类
	- 	使用 PHP 中的 instanceof 运算符，可以判断一个对象是否属于某一个类
	- 	使用 instanceof 也可用来确定一个对象是不是继承自某个父类的子类
	- 	instanceof 也可用于确定一个变量是不是实现了某个接口的对象的实例，
	- 	虽然 instanceof 通常是直接与类名一起使用，但也可以使用字符串来代替，使用字符串代替类名时，需要将字符串赋值给一个变量，直接使用字符串（例如 $obj instanceof 'A'）是不行的。
	- 	如果被检测的变量不是对象，instanceof 并不会报错而是直接返回 FALSE。另外，不能使用 instanceof 来检测常量。
	```
	class Demo {}
	class SubDemo extends Demo {}
	$obj = new Demo;
	$objCopy = clone $obj;
	$objDemo = new SubDemo;
	var_dump($obj instanceof Demo);
	var_dump($obj instanceof SubDemo);
	var_dump($objCopy instanceof Demo);
	var_dump($objDemo instanceof SubDemo);
	var_dump($objDemo instanceof Demo);
	```
*	PHP自动加载机制
	- 	在使用面向对象模式开发程序时，通常大家习惯为每个类都创建一个单独的 PHP 源文件。这样会很容易实现对类进行复用，同时将来维护时也很便利，这也是面向对象程序设计的基本思想之一。
	- 	在 PHP5 之前，当需要使用一个类时，只需要直接使用 include 或 require 将其包含进来即可。如果一个页面需要使用多个类，就不得不在脚本页面开头编写一个长长的包含文件的列表。将本页面需要的类文件全部包含进来，这样处理不仅烦琐，而且容易出错。
	- 	PHP 提供了 spl_autoload_register() 和 \_\_autoload() 函数来实现类的自动加载功能，这可以节省编程的时间。
	- 	\_\_autoload()函数自动加载类
		+	\_\_autoload()是系统函数，函数名是固定的，而且该函数没有方法体，需要我们自定义方法体。另外，该函数是自动调用的，当我们new一个类时，如果当前源文件中找不到这个类，php则会自动调用\_\_autoload()函数，并将类名传递给\_\_autoload()函数。还有一点需要注意的是，\_\_autoload()函数在当前源文件中只能定义一次
		+	想要使用 \_\_autoload() 函数自动加载类文件，类文件的名称需要与类名相同，另外一个类文件中只能定义一个类。
		+	\_\_autoload() 函数自 PHP7.2.0 起已被弃用，可以使用 spl_autoload_register() 函数代替。
		```
		function __autoload($className) {
			$file = './'.$className.'.php';
			include_once($file);
		}
		$obj = new Demo();
		```
	- 	spl_autoload_register()函数
		+	spl_autoload_register()函数可以指定一个函数来替代\_\_autoload()函数的功能。
		+	与 \_\_autoload() 函数不同，spl_autoload_register() 可以多次定义。
		```
		spl_autoload_register([$autoload_function [, $throw = true [, $prepend = false ]]]);
		参数说明如下：
		$autoload_function: 要替代__autoload()函数的函数名称，也可以是一个匿名函数。如果没有提供任何参数，则自动注册autoload的默认实现函数spl_autoload();
		$throw: 用来设置$autoload_function 无法成功注册时，spl_autoload_register() 函数是否抛出异常;
		$prepend: 如果是true，则spl_autoload_register()函数会添加$autoload_function函数到队列之首，否则添加到队列尾部。
		```
*	匿名类
	-	php7 支持通过 new class 来实例化一个匿名类，这可以用来替代一些“用后即焚”的完整类定义。
	```
	<?php
	interface Logger {
	    public function log(string $msg);
	}

	class Application {
	    private $logger;

	    public function getLogger(): Logger {
	         return $this->logger;
	    }

	    public function setLogger(Logger $logger) {
	         $this->logger = $logger;
	    }
	}

	$app = new Application;
	$app->setLogger(new class implements Logger {
	    public function log(string $msg) {
	        echo $msg;
	    }
	});

	var_dump($app->getLogger());
	?>
	```
*	PHP设计模式(编码规范)
	- 	设计模式不是一套具体的语言框架，而是一种行之有效的编码规范，是前人经过反复使用并总结出来的编写代码的经验。使用设计模式的目的是为了提高代码可重用性，让代码更容易被他人理解，同时保证代码可靠性。合理使用设计模式有助于我们更加深入地理解面向对象思维
	- 	工厂模式
		+	工厂模式是一种专门用来创建其它对象的类（称为“工厂类”），根据传递参数的不同，来创建不同类的对象。我们可以使用工厂类创建对象，而不是直接使用 new。
		+	工厂类中至少有一个公共的静态方法（称为“工厂方法”），静态方法接受一个参数，根据这个参数来创建不同类的对象。
	- 	单例模式
		+	单例模式也叫单子模式，是一种常用的软件设计模式。 在应用这个模式时，可以确保一个类只能创建一个对象，这么做可以极大节省内存空间，有利于我们协调系统的整体行为。
		+	使用单例模式创建的类（“单例类”）不能再其它类中直接实例化，只能被其自身实例化。它不会创建实例副本，而是会向单例类内部存储的实例返回一个引用。
		+	单例模式一个主要应用场合就是应用程序与数据库打交道的场景，在一个应用中会存在大量的数据库操作，针对数据库句柄连接数据库的行为，使用单例模式可以避免大量的 new 操作，因为每一次 new 操作都会消耗系统和内存的资源。
		+	实现单例模式的思路（三私一公）：私有的静态的对象实例；私有的构造方法，在类外不能使用 new 创建对象；私有的克隆方法，在类外不能使用 clone 克隆对象；公共的静态的创建对象实例的方法。
*	php魔术方法
	- 	在面向对象编程中，PHP 提供了一系列的魔术方法，这些魔术方法为编程提供了很多便利，在 PHP 中的作用是非常重要的。PHP 中的魔术方法通常以\_\_（两个下划线）开始，并且不需要显式的调用而是在某种特定条件下自动调用的。
	- 	\_\_construct()，实例化类的时候自动调用
	- 	\_\_destruct()，类对象使用结束时自动调用
	- 	\_\_set()，在给未定义的属性赋值时自动调用
		+	在为当前环境下未定义或不可见的类属性赋值时，会自动调用 \_\_set($name, $value) 方法。
		+	利用 \_\_set() 方法的特性，我们可以通过 \_\_set() 方法为类中的使用 private 关键词修饰的属性进行赋值或修改。
	- 	\_\_get()，在调用未定义的属性时自动调用
		+	在调用或获取当前环境下未定义或不可见的类属性时，会自动调用 \_\_get($name) 方法，
	- 	\_\_isset()，使用isset()和empty()函数时自动调用
		+	当在类外部对类中不可访问或不存在的属性使用 isset() 或 empty() 函数时，会自动调用 \_\_isset($name) 方法，
		+	类中的公有成员可以在类外访问，而私有成员则无法在类外访问。也就是说，我们可以使用 isset() 或 empty() 函数来检查类中的公有属性是否存在，而对类中的私有属性这两个函数就无效了。
		+	想要使用 isset() 或 empty() 函数对类中的私有属性进行检测的话，我们只需要在类中添加一个 \_\_isset() 方法就可以了，当在类外部使用 isset() 或 empty() 函数时，会自动调用类里面的 \_\_isset() 方法。
	- 	\_\_unset()，使用unset()函数时自动调用
		+	当在类外部对类中不可访问或不存在的属性使用 unset($name) 函数时，\_\_unset() 方法会被自动调用，
		+	我们也可以使用 unset() 函数在类外部去删除类中的成员属性。与上面介绍的 \_\_isset() 方法相似，如果要删除类中的公有属性的话直接使用 unset() 函数即可；如果要删除类中的私有属性的话，则需要在类中添加一个 \_\_unset() 方法。
	- 	\_\_sleep()，使用serialize序列化时自动调用
	- 	\_\_wakeup()，使用unserialize反序列化时自动调用
	- 	\_\_call()，调用一个不存在方法时自动调用
		+	当调用类中一个不可访问或不存在的方法时，\_\_call($func, $arguments) 方法会被调用。$name 为要调用的方法名称，$arguments 为传递给 $name 的参数所组成的数组。
		+	当调用的方法不存在时会自动调用 \_\_call() 方法，程序会继续执行下去，从而可以避免当调用方法不存在时产生错误所导致的程序终止。
	- 	\_\_callStatic()，调用一个不存在的静态方法时自动调用
	- 	\_\_toString()，把对象转换成字符串的时候自动调用
	- 	\_\_invoke()，当尝试把对象当方法调用时自动调用
	- 	\_\_set_state()，当使用var_export()函数时自动调用
	- 	\_\_clone()，当使用clone	复制一个对象时自动调用
		+	可以使用 clone 关键字复制对象，当复制完成时，如果定义了 \_\_clone() 方法，则新创建的对象（复制生成的对象）中的 \_\_clone() 方法会被自动调用，通过该方法我们可以做一些必要的操作。
	- 	\_\_debugInfo()，当使用var_dump()打印对象信息时自动调用
*	常用函数
	- 	isset()，可以检查一个变量是否存在并且不为 NULL，传入一个变量作为参数，如果传入的变量存在则传回 true，否则传回 false。
	- 	empty()，可以检查一个变量是否为空，同样需要传入一个变量作为参数，如果变量并不存在，或者变量的值等于 FALSE，那么这个变量会被认为不存在。
	- 	unset()，unset() 函数的作用是删除指定的变量，需要传入一个或多个变量作为参数，另外，该函数没有返回值。
	- 	property_exists()，可以用来检测类中是否定义了该属性，语法格式为 property_exists($class_name，$property_name)，其中 $class_name 为字符串形式的类名，即判断类 class_name 中是否定义了 property_name 属性。
	- 	count()，计算元素（数组、对象）的长度。
	- 	in_array()，判断元素是否在数组中
	- 	str_split()，字符串转数组
	- 	explode()，字符串转数组
	- 	implode()，数组转字符串
	- 	array_intersect()，函数用于比较两个（或更多个）数组的值，并返回一个交集数组。
	- 	array_diff()，比较两个数组的键值，并返回差集，array_diff($array1, $array2); 不管这两个数组是否相同都有可能返回的是空数组，因为它只返回 $array1 的差集，所以要验证是否相同的要相互比较才行，
	```PHP
	if ( !array_diff($arr1, $arr2) && !array_diff($arr2, $arr1) ) {
		// 即相互都不存在差集，那么这两个数组就是相同的了，多数组也一样的道理
	    return true;
	}
	```
	- 	array_search()，在数组中搜索某个键值，并返回对应的键名。
	- 	array_count_values()，对数组中所有的值进行计数。
	- 	intval()，浮点值取整数部分。
	- 	round()，浮点数取两位小数
	- 	ceil()，四舍五入向上取整。
	- 	floor()，四舍五入向下取整。
	- 	max()，返回数组中最大的数。
	-   array_sum()，返回数组中所有值的和。
	- 	array_map()，返回对数组中执行function中的操作后的数组
	```PHP
	array_map(function $func, $arr1 [, $arr2 [, $arr3]]);
	// 对传入的$nums，中的每个value执行函数array_sum，并把结果传入到新的数组中，并返回该数组，max函数用于计算传入数组中最大的值
	return max(array_map('array_sum', $nums));
	// 以对应的数组下标（key）的$arr1为key，和以$arr2为value，作为一个关联数组，存储到新的二维数组中并返回该二维数组
	array_map(num, $arr1, $arr2);
	// 自定义函数需要有返回值
	function func($v) {
		$v = 0 ? 1 : 0;  //  php7 $v ?? 1 : 0;
		return $v;
	}
	```

### php中的时间和日期
*	设置时区
	- 	在 PHP 中是通过日期和时间函数来获取日期和时间的。日期和时间函数依赖于服务器的时间设置，服务器的时间设置默认是格林尼治时间（零时区时间），如果不特意设置时间为特定时区时间，那么通过 PHP 有关函数获取到的时间为零时区的时间，比北京时间少 8 个小时。
	-	全球分为 24 个时区，每个时区都有自己的本地时间，同一时间内各时区的本地时间相差 1\~23 小时，如英国伦敦本地时间与北京本地时间相差 8 个小时。
	-	在国际无线电通信领域，使用一个统一的时间，称为通用协调时间（Universal Time Coordinated，UTC），UTC 与格林威治标准时间（Greenwich Mean Time，GMT）相同。
	-	直接修改php配置文件php.ini的date.timezone参数。`date.timezone = Asia/Shanghai;`
	-	使用函数ini_set()设置date.timezone参数。`ini_set('date.timezone', 'Asia/Hong_Kong');`
	-	使用函数date_default_timezone_set();`date_default_timezone_set('GMT');`
*	获取当前时间
	-	在日期和时间函数中，UNIX 时间戳的获取非常重要，时间戳是一个字符序列，是指格林尼治时间 1970年 01 月 01 日 00 时 00 分 00 秒（北京时间 1970 年 01 月 01 日 08 时 00 分 00 秒）起至现在的总毫秒数。
	-	gmmktime()，获取 GMT 日期的 UNIX 时间戳
	```
	int gmmktime([int $hour [, int $minute [, int $second [, int $month [, int $day [, int $year [, int $is_dst]]]]]]]);
	该函数的参数可以从右到左依次空着，空着的参数会被设为相应的当前 GMT 值。
	```
	-	mktime()，取得一个日期的 UNIX 时间戳。
	```
	int mktime([int $hour = date('H') [, int $minute = date('i') [, int $second = date('s') [, int $month = date('m') [, int $day = date('d') [, int $year = date('Y') [, int $is_dst = -1]]]]]]]);
	该函数根据给出的参数返回 UNIX 时间戳。时间戳是一个长整数，包含了从 UNIX 纪元到给定时间的秒数。
	和 gmmktime() 函数一样，该函数的参数也可以从右向左省略，任何省略的参数会被设置成本地日期和时间的当前值。
	```
	-	microtime()，获得当前 UNIX 时间戳和微秒数。
	```
	mixed microtime([boon $get_as_float]);
	如果设置 get_as_float 参数值为 true，microtime() 将返回一个浮点数；若不带参数，则返回一个“msec sec”格式的字符串，其中 sec 是自 UNIX 纪元起到现在的秒数，msec 是微秒部分。字符串的两部分都是以秒为单位返回的。
	```
	-	time()，返回当前的 UNIX 时间戳`int time(void);`
	-	getdate()，获取时间日期信息。
	```
	array getdate([int $timestamp = time()]);
	该函数返回一个根据$timestamp得出的包含有日期时间信息的关联数组。如果没有给出时间戳，则默认为当前本地时间。
	返回的关联数组中的键名及元素说明：
	int seconds: 秒的数字表示
	int minutes: 分钟的数字表示
	int hours: 小时的数字表示
	int mday: 月份中的第几天的数字表示
	int wday: 星期中的第几天的数字表示
	int mon: 月份的数字表示
	int year: 完整的数字年份表示
	int yday: 一年中的第几天的数字表示
	string weekday: 星期及的完整文本表示
	string month: 月份的完整文本表示
	int 0: 自从 UNIX 纪元开始至今的秒数，和 time() 的返回值以及用于 date() 的值类似	
	```
*	时间日期格式化
	- 	当时间和日期保存在计算机中的时候，可以使用 UNIX 时间戳作为标准格式。但是 UNIX 时间戳的可读性很差，所以有时我们需要把 UNIX 时间戳格式化为可读性更好的时间和日期，或者格式化为其它软件需要的格式。
	-	date()，格式化一个本地时间或者日期
	```
	string date(string $format [, int $timestamp]);
	’参数说明如下：
	$format：表示格式化后的时间格式，可以包含一些具有特殊含义的字符。
	$timestamp：表示待格式化的时间戳，是一个可选参数，默认为当前时间。也可以理解为 $timestamp 的默认值为 time()。
	```
*	计算时间差
	-	先转换为时间戳在进行算术操作。计算两个日期之间的时间差需要先把两个日期转换成纪元时间戳再计算，
	```
	$strTime = '2022-04-05 15:23:56';
	list($dateArr, $timeArr) = explode(' ', $strTime);
	list($year, $month, $day) = explode('-', $dateArr);
	list($hour, $minute, $second) = explode(':', $timeArr);
	$startTime = mktime($hour, $minute, $second, $month, $day, $year);;
	// sleep(5);
	$endTime = time();
	$seconds = $endTime - $startTime;
	$minutes = floor($seconds/60);
	$hours = floor($seconds/(60*60));
	$days = floor($seconds/(24*60*60));
	$weeks = floor($seconds/(24*60*60*7));

	echo date('Y-m-d H:i:s', $startTime).' 距离 '.date('Y-m-d H:i:s', $endTime).'已经过了：'.PHP_EOL;
	echo "{$seconds} 秒；\n";
	echo "{$minutes} 分钟； \n";
	echo "{$hours} 小时；\n";
	echo "{$days} 天；\n";
	echo "{$weeks} 周；\n";
	```
*	从字符串中提取时间和计算时间差
	-	strtotime()，有两种用法：一种是将字符串形式的、用英文文本描述的日期时间解析为 UNIX 时间戳，一种是用来计算一些日期时间的间隔。
	```
	int strtotime(string $date [, int $time = time()]);
	time 为字符串形式的日期时间；
	now 规定用来计算返回值的时间戳。如果省略该参数，则使用当前时间。
	// 当前时间的时间戳，time()
	echo strtotime("now").PHP_EOL;
	// 当前时间的前一天的时间戳
	echo strtotime('-1 day').PHP_EOL;
	// 昨天零点开始的时间戳
	echo strtotime("yesterday").PHP_EOL;
	// 今天零点的时间戳
	echo strtotime('today').PHP_EOL;
	echo strtotime("tomorrow").PHP_EOL;
	echo strtotime("today") - strtotime("yesterday").PHP_EOL;
	echo strtotime("now") - strtotime("today").PHP_EOL;
	echo strtotime('tomorrow').PHP_EOL;
	echo strtotime('+1 day').PHP_EOL;
	echo strtotime('+1 week 2 days 3 hours 4 minutes 5 seconds').PHP_EOL;
	echo strtotime('last Thursday').PHP_EOL;
	echo strtotime('next Friday').PHP_EOL;
	echo strtotime('July 9th, 2015').PHP_EOL;
	echo strtotime('Sept. 09 2014').PHP_EOL;
	echo strtotime('20120121').PHP_EOL;
	echo strtotime('120913').PHP_EOL;

	// 在一个日期上加减一定的时间间隔。可以使用 strtotime() 来计算一些日期时间间隔。
	$startTime = 'last Monday';
	$endTime = strtotime("{$startTime} + 7 day");
	// echo date('l', $endTime).PHP_EOL;
	echo "The deadline is : ".date('Y-m-d H:i:s', $endTime).PHP_EOL;

	// 如果日期使用时间戳表示，并且时间间隔也可用秒来表示，就可以从时间戳中减去时间间隔。
	$sTime = time();
	$interval = 24*60*60*7;
	$eTime = $sTime + $interval;
	echo date('Y-m-d', $eTime).PHP_EOL;
	```
*	检验日期和时间是否有效
	-	checkdate()，来检验日期和时间的有效性，
	```
	bool checkdate(int $month, int $day, int $year);
	month 的值是从 1 到 13，day 的值在给定的 month 所应该具有的天数范围之内，闰年也考虑进去，year 的值是从 1 到 32767。
	如果给出的日期有效就返回 true，否则返回 false。
	```
*	获取当前时间戳
	- 	在 UNIX 系统中，日期与时间表示为自 1970 年 01 月 01 日 00 时 00 分 00 秒（北京时间 1970 年 01 月 01 日 08 时 00 分 00 秒）起到当前时刻的总秒数，这种时间称为 UNIX 时间戳
	- 	UNIX 时间截提供了一种统一、简洁的时间表示方式，在不同的操作系统中均支持这种时间表示方式，同一时间在 UNIX 和 Windows 系统中均以相同的 UNIX 时间戳表示，所以不需要在不同的系统中进行转换。同时，UNIX 时间戳是一个时间差，与时区没有关系，无论当前 PHP 中使用的是何种时区，其 UNIX 时间戳是唯一的。
	-	time();`time();`
*	日期转时间戳
	-	strtotime()，可以将任何字符串类型的日期/时间转换为 UNIX 时间戳
	-	mktime()，来获取指定日期的 UNIX 时间戳，
*	php实现倒计时

### PHP正则表达式
*	正则表达式
	-	正则表达式是对字符串操作的一种逻辑公式，就是用事先定义好的一些特定字符、及这些特定字符的组合，组成一个“规则字符串”，这个“规则字符串”用来表达对字符串的一种过滤逻辑。比如注册用户时常常需要用户填写 E-mail、电话、生日之类的有特定格式的数据，为了避免用户胡乱填一些无效的数据，这时候就要用到正则表达式。
	-	正则表达式也称为模式表达式，自身具有一套非常完整的、可以编写模式的语法体系，提供了一种灵活且直观的字符串处理方法。正则表达式通过构建具有特定规则的模式，与输入的字符串信息比较，在特定的函数中使用从而实现字符串的匹配、查找、替换及分割等操作。
	```
	/http(s)?:\/\/[\w.]+[\w\/]*[\w.]*\??[\w=&\+\%]*/is      // 匹配网址 URL 的正则表达式
	/^\w{3,}@([a-z]{2,7}|[0-9]{3})\.(com|cn)$/                    // 匹配邮箱地址的正则表达式
	```
	- 	正则表达式描述的是一种字符串匹配模式，可以用来检查一个字符串中是否含有某种子串、将匹配的子串做替换或者从某个字符串中取出符合某个条件的子串等等。
	-	正则表达式是由普通字符（例如字符 a 到 z）以及特殊字符（称为“元字符”）组成的文字模式。正则表达式作为一个模板，将某个字符模式与所搜索的字符串进行匹配。正则表达式的模式可以是单个的字符、字符集合、字符范围、字符间的选择或者所有这些组件的任意组合。
*	正则表达式中的常用术语
	1.	grep, 最初是 ED 编辑器中的一条命令，用来显示文件中特定的内容。后来成为一个独立的工具 grep。
	2.	egrep, grep 虽然不断地更新升级，但仍然无法跟上技术的脚步。为此，贝尔实验室写出了 egrep，意为“扩展的 grep"。这大大增强了正则表达式的能力。
	3.	POSIX（Portable Operating System Interface of UNIX）, 可移植操作系统接口。在 grep 发展的同时，其他一些开发人员也根据自己的喜好开发出了具有独特风格的版本。但问题也随之而来，有的程序支持某个元字符，而有的程序则不支持。因此，就有了POSIX。POSIX 是一系列标准，确保了操作系统之间的移植性。不过 POSIX 和 SQL 一样，没有成为最终的标准而只能作为一个参考。
	4.	Perl（Practical Extraction and Reporting Language）, 实际抽取与汇报语言。1987 年，Larry Wall 发布了 Perl。在随后的 7 年时间里，从 Perl1 到现在的 Perl5，最终成为了 POSIX 之后的另一个标准。
	5.	PCRE, Perl 的成功，让其他的开发人员在某种程度上要兼容"Perl"，包括 C/C++、Java、Python 等都有自己的正则表达式。1997 年，Philip Hazel 开发了 PCRE 库，这是兼容 Perl 正则表达式的一套正则引擎，其他开发人员可以将 PCRE 整合到自己的语言中，为用户提供丰富的正则功能。许多软件都使用 PCRE，PHP 正是其中的一员。
*	正则表达式语法规则
	-	正则表达式的构成元素中一般包括普通字符、元字符、限定符、定位点、非打印字符和指定替换项等。
	-	普通字符，普通字符包括没有显式指定为元字符的所有可打印和不可打印字符，包括所有大小写字母、数字、标点符号和一些符号。最简单的正则表达式是用于搜索字符串相比较的单个普通字符。例如，单字符正则表达式/A/会始终匹配字母 A。也可以将多个单字符组合起来形成较长的表达式，例如，正则表达式/the/会匹配搜索字符串中的 the、there、other 和 over the lazy dog 等。无须使用任何串联运算符，只需连续输入字符即可。
	-	元字符

	|		元字符		|													行为												|													示例													|
	|		  ---			|													 ---												|													  ---													|
	|			*			|	零次或多次匹配之前的字符或子表达式，等效于{0, }						|	zo* 与 “z”和“zoo”匹配																		|
	|			+			|	一次或多次匹配之前的字符或子表达式，等效于{1, }						|	zo+ 与 “zo”和“zoo”匹配，但与“z”不匹配												|
	|			?			|	零次或一次匹配之前的字符或子表达式，等效于{0, 1} \n 当 ? 紧随任何其他限定符（\*、\+、?、{n}、{n,} 或 {n,m}）之后时，匹配模式是非贪婪的。非贪婪模式匹配搜索到的、尽可能少的字符串，而默认的贪婪模式匹配搜索到的、尽可能多的字符串						|	zo? 与“z”和“zo”匹配，但与“zoo”不匹配 \n o+? 只与“oooo”中的单个“o”匹配，而 o+ 与所有“o”匹配 \n do(es)? 与“do”或“does”中的“do”匹配	|
	|			^			|	匹配搜索字符串开始的位置。如果标志中包括 m（多行搜索）字符，^ 还将匹配 \n 或 \r 后面的位置。如果将 ^ 用作括号表达式中的第一个字符，就会对字符集取反	|	^\d{3} 与搜索字符串开始处的 3 个字符匹配 \n [^abc] 与除 a、b、c 以外的任何字符匹配	|
	|			$			|		匹配搜索字符串结束的位置。如果标志中包括 m（多行搜索）字符，^ 还将匹配 \n 或 \r 前面的位置。	|	\d{3}$ 与搜索字符串结尾处的 3 个数字匹配	|
	|			.			|	匹配除换行符 \n 之外的任何单个字符。若要匹配包括 \n 在内的任意字符，请使用诸如 [\s\S] 之类的模式	|	a.c 与 “abc”“a1c”和“a-c”匹配	|
	|			[]			|	标记括号表达式的开始和结尾															|	[1-4] 与“1”、“2”、“3”或“4”匹配  \n [^aAeEiIoOuU] 与任何非元音字符匹配	|
	|			{}			|	标记限定符表达式的开始和结尾														|	a{2,3} 与“aa”和“aaa”匹配																		|
	|			()			|	标记子表达式的开始和结尾，可以保存子表达式，以备将来之用		|	A(\d) 与“A0”至“A9”匹配。保存该数字以备将来之用								|
	|			|			|	指示两个或多个项之间进行选择														|	z|food 与“z”或“food”匹配  \n (z|f)ood 与 “zood”或“food”匹配				|
	|			/			|	表示 JavaScript 中的文本正则表达式模式的开始和结尾。在第二个 “/”后添加单字符标志可以指定搜索行为	|	/abc/gi 是与 “abc”匹配的 JavaScript 文本正则表达式。g（全局）标志指定查找模式的所有匹配项，i（忽略大小写）标志使搜索不区分大小写	|
	|			\			|		将下一字符标记为特殊字符、文本、反向引用或八进制转义符		|	\n 与换行符匹配。\( 与 “(”匹配。\\ 与 “\”匹配	|

	- 	这些特殊字符在括号表达式内出现时就会失去它们的意义，变成普通字符。若要匹配这些特殊字符，必须首先转义字符，即在字符前面加反斜杠\。例如，若要搜索\+文本字符，则可使用表达式\+。
	- 	除了以上单字符元字符外，还有一些多字符元字符，
	- 	非打印字符
	- 	优先级顺序
		+	在使用正则表达式时，需要注意匹配的顺序。通常相同优先级是从左到右进行运算的，不同优先级的运算先高后低。各种操作符的匹配顺序优先级从高到低，
		+	另外，字符具有高于替换运算符的优先级，例如，允许 "m|food" 匹配 "m" 或 "food"。

	|	顺序	|					元字符					|					描述						|
	|	---	|						---					|					---						|
	|	1		|						\						|					转义符					|
	|	2		|				( )、(?:)、(?=)、[ ]		|				括号和中括号			|
	|	3		|		\*、+、{n}、{n，}、{n,m}		|					限定符					|
	|	4		|			^、$、\ 任何元字符		|				定位点和序列			|
	|	5		|						|						|						替换					|

	- 	替换
		+	正则表达式中的替换允许对两个或多个替换选项之间的选择进行分组。实际上可以在模式中指定两种匹配模式的或关系。可以使用管道|字符指定两个或多个替换选项之间的选择，称之为“替换”。匹配管道字符任一侧最大的表达式。
		```
		/Chatper|Section [1-9][0-9]{0,1}}/
		该正则表达式匹配的是字符串“Chapter”或者字符串“Section”后跟一个或两个数字。
		如果搜索字符串是“Section 22”，那么该表达式匹配“Section 22”。但是，如果搜索字符串是“Chapter 22”，
		那么表达式匹配单词“Chapter”，而不是匹配“Chapter 22”。
		为了解决这种形式的表达式可能带来的误导，可以使用括号来限制替换的范围，即确保它只应用于两个单词“Chapter”和“Section”。
		可以通过添加括号来使正则表达式匹配“Chapter 1”或“Section 3”。将以上表达式改成如下形式：
		/(Chatper|Section) [1-9][0-9]{0,1}/
		修改后，如果搜索字符串是“Section 22”，那么该表达式匹配“Section 22”。
		如果搜索字符串是“Chapter 22”，那么表达式匹配单词也会是“Chapter 22”。
		```
	- 	子表达式
		+	正则表达式中放置括号可创建子表达式，子表达式允许匹配搜索文本中的模式并将匹配项分成多个单独的子匹配项，程序可检索生成的子匹配项。例如匹配邮箱账号的正则表达式：
		```
		/(\w+)@(\w+)\.(\w+)/
		该正则表达式包含 3 个子表达式，3 个子表达式分别进行匹配并保留匹配结果，与其他表达式匹配结果作为一个整体显示出来。
		下面的示例将通用资源指示符（URI）分解为其组件：
		/(\w+):\/\/([^\/:]+)(:\d*)?([^# ]*)/
		第一个括号子表达式保存 Web 地址的协议部分，匹配在冒号和两个正斜杠前面的任何单词。	
		第二个括号子表达式保存地址的域地址部分，匹配不包括左斜线/或冒号：字符的任何字符序列。
		第三个括号子表达式保存网站端口号（如果指定了的话），匹配冒号后面的零个或多个数字。
		第四个括号子表达式保存 Web 地址指定的路径和/或页信息，匹配零个或多个数字字符#或空白字符之外的字符。
		如果我们使用这个正则表达式匹配字符串“http://msdn.microsoft.com:80/scripting/default.htm”，
		那么 3 个子表达式的匹配结果分别为 http、msdn.microsoft.com:80、/scripting/default.htm。
		```
	- 	反向引用
		+	反向引用用于查找重复字符组。此外，可使用反向引用来重新排列输入字符串中各个元素的顺序和位置，以重新设置输入字符串的格式。
		+	可以从正则表达式和替换字符串中引用子表达式。每个子表达式都由一个编号来标识，并称作反向引用。
		+	在正则表达式中，每个保存的子匹配项按照它们从左到右出现的顺序存储。用于存储子匹配项的缓冲区编号从 1 开始，最多可存储 99 个子表达式。在正则表达式中，可以使用 \n 来访问每个缓冲区，其中 n 标识特定缓冲区的一位或两位十进制数字。
		```
		反向引用的一个应用是，提供查找文本中两个相同单词的匹配项的能力。以下面的句子为例：
		Is is the cost of of gasoline going up up?
		该句子包含多个重复的单词。如果能设计一种方法定位该句子，而不必查找每个单词的重复出现，就会很有用。
		下面的正则表达式使用单个子表达式来实现这一点：
		/\b([a-z]+) \1\b/
		在此情况下，子表达式是括在括号中的所有内容。该子表达式包括由 [a-z]+ 指定的一个或多个字母字符。
		正则表达式的第二部分是对以前保存的子匹配项的引用，即单词的第二个匹配项正好由括号表达式匹配。
		\1 用于指定第一个子匹配项。\b 单词边界元字符确保只检测单独的单词。否则，
		诸如“is issued”或“this is”之类的词组将不能正确地被此表达式识别。
		所以，使用表达式 /\b([a-z]+)\1\b/ 匹配字符串“Is is the cost of of gasoline going up up？”得到的结果为 is、of、up。
		```
*	在php中使用正则表达式
	+	php有两套函数库支持正则表达式处理操作
		1.	PCRE(Perl Compatible Regular Expression)库提供，与Perl语言兼容的正则表达式函数，以preg_为函数的前缀名称。
		2.	POSIX(Portable Operating System Interface)扩展语法正则表达式函数，以ereg_为函数前缀。
	+	两套函数库的功能相似，但是 PCRE 的执行效率高于 POSIX，所以我们只介绍 PCRE 函数库。
*	php中的preg_函数
	- 	preg_match()，根据正则表达式对字符串进行搜索匹配。
	```
	preg_match($pattern, $subject [, &$matches [, $flags = 0 [, $offset = 0]]]);
	参数说明如下：
	$pattern：要搜索的模式，也就是编辑好的正则表达式；
	$subject：要搜索的字符串；
	$matches：可选参数（数组类型），如果提供了 $matches，它将被填充为搜索结果。 $matches[0] 包含完整模式匹配到的文本， $matches[1] 包含第一个捕获子组匹配到的文本，以此类推；
	$flags：可选参数，$flags 可以被设置为 PREG_OFFSET_CAPTURE，如果传递了这个标记，对于每一个出现的匹配，返回时都会附加上字符串偏移量（相对于目标字符串的）；
	$offset：可选参数，用于指定从目标字符串的哪个位置开始搜索（单位是字节）。
	preg_match() 函数可以返回 $pattern 的匹配次数，它的值将是 0 次（不匹配）或 1 次，因为 preg_match() 在第一次匹配后将会停止搜索。
	```
	- 	preg_match_all()，搜索所有可以和正则表达式匹配的结果。
	```
	preg_match_all($pattern, $subject [, &matches [, $flags = PREG_PATTERN_ORDER [, $offset = 0]]]);
	参数说明如下：
	$pattern：要搜索的模式，也就是定义好的正则表达式；
	$subject：要搜索的字符串；
	$matches：可选参数（多维数组），用来存放所有匹配的结果, 数组排序通过 $flags 指定；
	$flags：可选参数，可以结合下面几个标记使用（注意不能同时使用 PREG_PATTERN_ORDER 和 PREG_SET_ORDER）：
	PREG_PATTERN_ORDER：结果排序为 $matches[0] 保存完整模式的所有匹配，$matches[1] 保存第一个子组的所有匹配，以此类推。
	PREG_SET_ORDER：结果排序为 $matches[0] 包含第一次匹配得到的所有匹配（包含子组）， $matches[1] 是包含第二次匹配到的所有匹配(包含子组)的数组，以此类推。
	PREG_OFFSET_CAPTURE：如果这个标记被传递，每个发现的匹配返回时会增加它相对目标字符串的偏移量。注意这会改变 $matches 中的每一个匹配结果字符串元素，使其成为一个第 0 个元素为匹配结果字符串，第 1 个元素为匹配结果字符串在 subject 中的偏移量。
	$offset：可选参数，$offset 用于从目标字符串中指定位置开始搜索（单位是字节）。
	preg_match_all() 函数可以返回 $pattern 的匹配次数（可能是 0），如果发生错误则返回 FALSE。
	```
	- 	使用正则表达式除了可以匹配字符串外，还可以匹配数组中的元素。PHP中的preg_grep()函数可以搜素数组中的所有元素，并返回与正则表达式匹配的所有元素所组成的数组。
	```
	preg_grep($pattern, $input [, $flags = 0]);
	参数说明如下：
	$pattern：要搜索的模式，也就是定义好的正则表达式；
	$input：要搜索的数组。
	$flags：可选参数，可以设置为PREG_GREP_INVERT，这时函数会返回数组中给定模式$pattern不匹配的元素组成的数组。
	preg_grep()函数将遍历$input数组中的每一个元素，让该元素与模式$pattern进行匹配，然后将匹配成功或者匹配失败的元素返回。
	```
	- 	grep_replace()，执行一个正则表达式的搜索和替换
	```
	grep_replace($pattern, $replacement, $subject [, limit = -1 [, &$count]]);
	参数说明如下：
	$pattern：要搜索的模式，可以使一个字符串或字符串数组；
	$replacement：用于替换的字符串或字符串数组。如果这个参数是一个字符串，并且 $pattern 是一个数组，那么所有的模式
	都使用这个字符串进行替换。如果 $pattern 和 $replacement 都是数组，每个 $pattern 使用 $replacement 中对应的元素
	进行替换。如果 $replacement 中的元素比 $pattern 中的少，多出来的 $pattern 使用空字符串进行替换。
	$subject：要进行搜索和替换的字符串或字符串数组，如果 $subject 是一个数组，搜索和替换回在 $subject 的每一个元素
	上进行, 并且返回值也会是一个数组。
	$limit：可选参数，每个模式在每个 $subject 上进行替换的最大次数。默认是 -1（无限）。
	$count：可选参数，如果指定，将会被填充为完成的替换次数。

	如果 $subject 是一个数组，preg_replace() 函数会返回一个数组，其他情况下返回一个字符串。

	如果函数 preg_replace() 搜索到匹配项，则会返回被替换后的 $subject，否则返回没有改变的 $subject。
	preg_replace() 函数的每个参数（除了参数 $limit）都可以是一个数组。如果参数 $pattern 和参数 $replacement 都是数组，
	那么该函数将以其键名在数组中出现的顺序来进行处理。如果发生错误，则返回 NULL。

	参数 $replacement 中可以包含后向引用 \\n 或 $n，语法上首选后者。每个这样的引用将被匹配到的第 n 个捕获子组捕获到的文本替换。
	n 可以是 0-99，\\0 和 $0 代表完整的模式匹配文本。

	捕获子组的序号计数方式为：代表捕获子组的左括号从左到右，从 1 开始数。如果要在 $replacement 中使用反斜线，
	必须使用 4 个（"\\\\" 因为这首先是 php 的字符串，经过转义后是两个，再经过正则表达式引擎后才被认为是一个原文反斜线）。

	当在替换模式下工作并且后向引用后面紧跟着需要是另外一个数字（比如：在一个匹配模式后紧接着增加一个原文数字），
	不能使用 \\1 这样的语法来描述后向引用。比如，\\11 将会使 preg_replace() 不能理解你希望的是一个 \\1 后向引用紧跟一个原文 1，
	还是一个 \\11 后向引用后面不跟任何东西。这种情况下解决方案是使用 ${1}1。这创建了一个独立的 $1 后向引用，一个独立的原文 1。

	当使用被弃用的 e 修饰符时，这个函数会转义一些字符（即：'、"、\ 和 NULL）然后进行后向引用替换。
	当这些完成后请确保后向引用解析完后没有单引号或双引号引起的语法错误（比如：'strlen(\'$1\')+strlen("$2")')。确保符合 PHP 的字符串语法，
	并且符合 eval 语法。因为在完成替换后，引擎会将结果字符串作为 php 代码使用 eval 方式进行评估并将返回值作为最终参与替换的字符串。
	```
	- 	preg_filter()，PHP preg_filter() 函数也用于执行一个正则表达式的搜索和替换，等价于我们前面介绍的《preg_replace() 函数》，不同的是 preg_filter() 函数只返回匹配成功的结果，而 preg_replace() 返回所有结果，不管是否匹配成功。
	- 	preg_split()，通过一个正则表达式来分割字符串，
	```
	array preg_split(string $pattern, string $subject [, int $limit = -1 [, int $flags = 0 ]]);
	参数说明如下：
	pattern：用于匹配的模式，也即正则表达式。
	subject　要分隔的字符串。
	limit：可选参数，如果指定，就将限制分隔得到的子串最多只有 limit 个，并且最后一个子串将包含所有剩余部分。
	limit 值为 -1、0 或 NULL 时都代表“不限制”，建议使用 NULL。
	flags：可选参数，它有 3 个取值。
	若设置为 PREG_SPLIT_NO_EMPTY，则 preg_split() 将返回分隔后的非空部分。
	若设置为 PREG_SPLIT_DELIM_CAPTURE，则分隔的模式中的括号表达式将被捕获并返回。
	若设置为 PREG_SPLIT_OFFSET_CAPTURE，则对于每一个出现的匹配返回时会附加字符串偏移量。
	注意：这将会改变返回数组中的每一个元素，使每个元素成为一个由第 0 个元素为分隔后的子串、第 1 个元素为该子串在 subject 中的偏移量组成的数组。
	返回值：返回一个使用 pattern 分割 subject 字符串后得到的子串组成的数组。
	```
	- 	preg_quote()，用来对正则表达式字符串进行转义，也就是在特殊字符前边增加一个反斜杠\，
	```
	string preg_quote($str [, $delimiter = NULL]);
	参数说明如下：
	$str：正则表达式字符串；
	$delimiter：可选参数，额外增加的需要转义的字符。如果指定了 $delimiter 参数，被指定的字符也会被转义。
	这通常用于转义 PCRE 函数使用的分隔符。 / 是最常见的分隔符。
	preg_quote() 函数会向参数 $str 提供的每个正则表达式的字符前增加一个反斜线。这通常用于一些运行时字符串需要作为正则表达式进行匹配的时候。
	正则表达式特殊字符有：. \ + * ? [ ^ ] $ ( ) { } = ! < > | : -。
	注意：/不是正则表达式特殊字符。
	```

### 会话控制
* 	会话控制是一种面向连接的可靠通信方式，通常用来判断用户的登录行为。例如，当我们在某个 E-mail 系统上成功登录以后，在这之间的查看邮件、收信、发信等过程可能需要访问多个页面来完成，但在同一个系统上，多个页面之间互相切换时，还能保持用户登录的状态，并且访问的都是登录用户自己的信息。这种能够在网站中跟踪一个用户，并且可以处理在同一个网站中同一个用户在多个页面共享数据的机制，都需要使用会话控制来完成的。
* 	会话控制包括 Session 和 Cookie 两种技术。两者既有区别又有相通之处，它们的主要功能都是把客户端与服务器端有机地关联起来，以便能够有效管理和查看用户在网站中的状态。
* 	在同一个系统上，多个页面之间互相切换时，还能保持用户的登录状态，并且访问的都是用户自己的信息。这种能够在网站中跟踪一个用户，并且可以处理在同一个网站中同一个用户在多个页面共享数据的机制，都需要使用会话控制的思想完成。
* 	为什么要用到会话控制
	-	在浏览网页时，访问每一个 Web 页面都需要使用到“HTTP 协议”，而 HTTP 协议是无状态协议，也就是说 HTTP 协议没有一个内建机制来维护两个事务之间的状态。当一个用户请求一个页面以后，再请求同一个网站上的另外一个页面时，HTTP 协议不能告诉我们这两个请求是来自同一个用户，会被当做独立的请求，而并不会将这两次访问联系在一起
	-	会话控制的思想就是允许服务器跟踪同一个客户端做出的连续请求。这样，我们就可以很容易地做到用户登录的支持，而不是在每浏览一个网页都去重复执行登入的动作。当然，使用会话控制除了可以在同一个网站中跟踪 Web 用户之外，对同一个访问者的请求还可以在多个页面之间为其共享数据。
*	会话控制方式
	-	由于 HTTP 是无状态的协议，所以不能维护两个事务之间的状态。PHP 系统为了防止这种情况的发生，提供了如下三种网页之间传递数据的方法。
	-	使用超链接或者 header() 函数，并在 URL 的 GET 请求中附加参数的形式，将数据从一个页面转向另一个 PHP 脚本中。也可以通过网页中的各种隐藏表单来储存使用者的资料，并将这些信息在提交表单时传递给服务器中的 PHP 脚本；
	-	使用 Cookie 将用户的状态信息存放在浏览器中，并通过浏览器来存取 Cookie 中的信息；
	-	相对于 Cookie 还可以使用 Session，将访问者的状态信息存放于服务器之中，让其他程序能透过服务器中的文件或数据库，来存取使用者的信息。
	-	在上面三种网页间数据的传递方式之中，使用 URL 或表单的方式主要是用来处理参数的传递或是多条信息的输入，适合于两个脚本之间的简单数据传递。例如，通过表单修改或删除数据时，可以将数据对应的 ID 传递给其他脚本。
	-	如果需要传递的数据比较多，页面传递的次数比较频繁，或者是需要传递数组时，使用 URL 或表单就有些烦琐了。特别是在项目中跟踪一个用户时，要为不同权限的用户提供不同的动态页面，需要每个页面都知道现在的用户是谁，对于这种情况我们通常选用 Cookie 和 Session 技术。
*	cookie是什么
	-	Cookie 是在 HTTP 协议下，服务器或脚本用来维护客户端上信息的一种方式。Cookie 是由 Web 服务器保存在用户浏览器（客户端）上的小文本文件，它可以包含有关用户的信息。无论何时用户链接到服务器，Web 站点都可以访问 Cookie 信息。
	-	有些 Cookie 是临时的，有些则是持续的。临时的 Cookie 只在浏览器上保存一段规定的时间，一旦超过规定的时间，该 Cookie 就会被系统清除。
	-	持续的 Cookie 则保存在用户的 Cookie 文件中，下一次用户返回时，仍然可以对它进行调用。在 Cookie 文件中保存 Cookie，有些用户担心 Cookie 中的用户信息被一些别有用心的人窃取，而造成一定的损害。
	-	网站以外的用户无法跨过网站来获得 Cookie 信息。如果因为这种担心而屏蔽 Cookie，肯定会因此拒绝访问许多站点页面。因为，当今有许多 Web 站点开发人员使用 Cookie 技术，例如 Session 对象的使用就离不开 Cookie 的支持。
	-	Cookie 的主要用途
		+	服务器可以利用 Cookie 包含信息的任意性来筛选并维护这些信息，以判断 HTTP 传输中的状态。Cookie 最典型的应用是判定注册用户是否已经登录网站，同时用户可能也会得到提示，是否在下一次进入此网站时保留用户信息以便简化登录手续，这些都是 Cookie 的功用。
		+	另一个重要应用场合就是商城类网站的“购物车”功能。用户可能会在一段时间内在同一家网站的不同页面中选择不同的商品，这些信息都会写入 Cookie，以便在最后付款时提取信息。
	-	Cookie 的生命周期
		+	Cookie 可以保持登录信息到用户下次与服务器的会话，换句话说，下次访问同一网站时，用户会发现不必输入用户名和密码就已经登录了（当然，如果手动删除 Cookie 就需要重新登陆了）。而还有一些 Cookie 在用户退出会话的时候就被删除了，这样可以有效保护个人隐私。
		+	Cookie 在生成时就会被指定一个 Expire 值，这就是 Cookie 的生存周期，在这个周期内 Cookie 有效，超出这个周期 Cookie 就会被清除。有些页面将 Cookie 的生存周期设置为“0”或负值，这样在关闭浏览器时，就会马上清除 Cookie，不会记录用户信息，更加安全。
	-	Cookie 的识别功能
		+	如果在一台计算机中安装多个浏览器，每个浏览器都会在各自独立的空间存放 Cookie。因为 Cookie 中不但可以确认用户，还能包含计算机和浏览器的信息，所以一个用户用不同的浏览器登录或者用不同的计算机登录，都会得到不同的 Cookie 信息，另一方面，对于在同一台计算机上使用同一浏览器的多个用户，Cookie 不会区分他们的身份，除非他们使用不同的用户名登录。
	-	脚本攻击
		+	尽管 Cookie 没有病毒那么危险，但它仍包含了一些敏感信息，比如用户名、计算机名等，使用的浏览器和曾经访问的网站。用户不希望这些内容泄漏出去，尤其是当其中还包含有私人信息的时候。
		+	这并非危言耸听，一种名为跨站脚本攻击（Cross site scripting）的方式就可以达到此目的。通常跨站脚本攻击往往利用网站漏洞在网站页面中植入脚本代码或网站页面引用第三方法脚本代码，均存在跨站脚本攻击的可能，在受到跨站脚本攻击时，脚本指令将会读取当前站点的所有 Cookie 内容（已不存在 Cookie 作用域限制），然后通过某种方式将 Cookie 内容提交到指定的服务器（如：AJAX）。一旦 Cookie 落入攻击者手中，它将会重现其价值
		+	建议开发人员在向客户端 Cookie 输出敏感的内容时，要注意以下几点（譬如：该内容能识别用户身份）：
			1.	设置 Cookie 不能被脚本读取，这样在一定程度上可以解决上述问题；
			2.	对 Cookie 内容进行加密，在加密前嵌入时间戳，保证每次加密后的密文都不一样（并且可以防止消息重放）；
			3.	客户端请求时，每次或定时更新 Cookie 内容（即重新加密）；
			4.	每次向 Cookie 写入时间戳，数据库需要记录最后一次时间戳（防止 Cookie 篡改，或重放攻击）；
			5.	客户端提交 Cookie 时，先解密然后校验时间戳，时间戳若小于数据数据库中记录，即意味发生攻击。
		+	基于上述建议，即使 Cookie 被窃取，却因 Cookie 被随机更新，且内容无规律性，攻击者无法加以利用。另外利用了时间戳另一大好处就是防止 Cookie 篡改或重放。
*	cookie在浏览器中是如何储存的
	-	Cookie 是存储在客户端的一段数据，但是不同的浏览器存储 Cookie 的地方不同：一种是将 Cookie 数据保存在文件中；另一种是保存在浏览器内存中。
	- 	IE 浏览器，Windows 系统上 IE 浏览器 Cookie 数据位于 %APPDATA%\Microsoft\Windows\Cookies\ 目录中的 xxx.txt 文件，里面可能有很多个. txt Cookie 文件，如 C：\Users\yren9\AppData\Roaming\Microsoft\Windows\Cookies\0WQ6YROK.txt。IE 浏览器中，IE 将各个站点的 Cookie 分别保存为一个 XXX.txt 这样的纯文本文件；
	- 	Firefox 和 Chrome 浏览器
		+	Firefox 和 Chrome 是将所有的 Cookie 都保存在一个文件中，该文件的格式为 SQLite 数据库格式的文件。
		+	Firefox 的 Cookie 数据位于 %APPDATA%\Mozilla\Firefox\Profiles\ 目录中的 xxx.default 目录下，名为 Cookies.sqlite 的文件中，如 C：\Users\jay\AppData\Roaming\Mozilla\Firefox\Profiles\ji4grfex.default\cookies.sqlite。
		+	在 Firefox 中查看 Cookie，可以选择“工具>选项>隐私>显示 Cookie”。
		+	Chrome 的 Cookie 数据位于 %LOCALAPPDATA%\Google\Chrome\User Data\Default\ 目录中名为 Cookies 的文件中，如 C：\Users\jay\AppData\Local\Google\Chrome\User Data\Default\Cookies。
*	PHP设置cookie
	- 	php中使用setcookie()函数来设置一个cookie，不过在设置cookie之前必须了解的是，cookie作为http响应头的一部分，而响应头必须在页面其他内容之前发送，它必须最先输出。若在setcookie()函数前输出一个html标记或者是echo语句，都会报错。
	- 	setcookie(),
	```
	bool setcookie(string $name [, string $value = "" [, int $expire = 0 [, string $path = "" [, string $damain = "" [, bool $secure = false [, bool $httponly = false]]]]]]);
	参数说明如下：
	$name：设置 Cookie 的名称；
	$value：可选参数，用来设置 Cookie 的值。可以通过 $_COOKIE['$name'] 的形式来获取 $value 的值；
	$expire：可选参数，用来设置 Cookie 的过期时间，这个时间是 Unix 时间戳的形式。如果设置成零或者忽略该参数，Cookie 会在会话结束时过期（也就是关掉浏览器时）；
	$path：可选参数，用来设置 Cookie 有效的服务器路径。 设置成 '/' 时，Cookie 对整个域名 $domain 有效。 如果设置成'/foo/'，则 Cookie 仅仅对 $domain 中 /foo/ 目录及其子目录有效（比如 /foo/bar/）。默认值为设置 Cookie 时的目录；
	$domain：可选参数，用来设置 Cookie 的有效域名/子域名。设置成子域名（例如 'c.biancheng.net'），会使 Cookie 对这个子域名和它的三级域名有效（例如 php.c.biancheng.net）。 要让 Cookie 对整个域名有效（包括它的全部子域名），只要设置成域名就可以了（例如 'biancheng.net'）；
	$secure：可选参数，用来设置这个 Cookie 是否仅仅通过安全的 HTTPS 连接传给客户端。设置成 TRUE 时，只有安全连接存在时才会设置 Cookie；
	$httponly：可选参数，设置成 TRUE 时，Cookie 仅可通过 HTTP 协议访问，也就是说 Cookie 无法通过类似 JavaScript 这样的脚本语言访问。设置该参数可以有效的减少受到 XSS 攻击的风险。
	如果在调用 setcookie() 函数以前产生了输出，setcookie() 会调用失败并返回 FALSE。 如果 setcookie() 成功运行，则会返回 TRUE。
	```
*	PHP获取cookie的值
	-	如果 Cookie 设置成功，客户端就拥有了 Cookie 文件，用来保存 Web 服务器为其设置的用户信息。以 Chrome 浏览器为例，其 Cookie 文件就存放在“C:\Users\用户名\AppData\Local\Google\Chrome\User Data\Default”目录下的 Cookies 文件中。
	-	Cookie 是一个以文本形式记录信息的，当我们再次访问一个网站时，浏览器会自动把与该站点对应的 Cookie 信息全部发送给服务器。
	-	从 PHP5 之后，任何 Cookie 信息都会被自动保存在超全局变量 $\_COOKIE 中，所以在每个 PHP 脚本中都可以从 $\_COOKIE 中读取相应的 Cookie 信息。$\_COOKIE 中存储着所有通过 HTTP 传递的 Cookie 内容，并以 Cookie 的识别名称为索引值、内容值为元素。
	-	在设置 Cookie 的脚本中，第一次读取它的信息并不会生效，必须刷新或下一个执行脚本时才可以看到 Cookie 值，因为 Cookie 要先被设置到浏览器中，当再次访问时才能被发送过来，这时才能被获取。所以要测试一个 Cookie 是否被成功设定，可以再其到期之前通过另外一个页面来访问其的值。
*	PHP删除cookie的值
	-	当 Cookie 被创建后，如果没有设置它的失效时间，其 Cookie 文件会在关闭浏览器时被自动删除，如果要在关闭浏览器之前删除 Cookie 文件，同样需要使用 setcookie() 函数。
	-	要删除cookie的值，可使用setcookie()把对应的cookie值（第二个参数）设置为空，或者把cookie的过期时间设置为小于当前系统的时间即可。
*	PHP Cookie的优点与缺点
	-	Cookie 是 Web 服务器在用户访问 Internet 站点时传递到 Web 浏览器的消息。浏览器会将每条消息以键值对的形式存储在用户计算机上的一个小文件中。当用户从服务器请求另一个页面时，浏览器会将 Cookie 发送回服务器。这些文件通常包含有关用户访问网页的信息，以及一些用户自愿提供的信息，例如：用户名，用户首选项，记住密码选项等。
	-	Cookie 的优点
		+	Cookie 易于使用和实现
		+	占用内存少
		+	 持久性
		+	易于管理
	-	Cookie 的缺点
		+	隐私问题
		+	不安全
		+	大小有限制，只能储存简单字符串信息
		+	可以被禁用
*	Session是什么
	-	在 PHP 中，Session 是一种服务器端的机制，服务器使用一种散列表的结构（类似于 JSON）来保存信息。相比于保存在客户端的 Cookie，Session 将用户交互信息保存在了服务器端，使得同一个客户端每次和服务端交互时，不需要每次都传回所有的 Cookie 值，而是只需要传回一个 ID 即可，这个 ID 是客户端第一次访问服务器的时候生成的，而且是唯一的。还有一点就是，因为 Cookie 存储在客户端，所以用户有权禁用 Cookie，而 Session 是存储在服务器端的，用户无法禁用。
	-	Session 简介
		+	Session 在 Web 技术中占有非常重要的地位。由于网页是一种无状态的连接程序，无法记录用户的浏览状态，所以需要通过 Session 来记录用户的有关信息，以供用户再次以这个身份对 Web 服务器发起请求。
		+	Session 中文是“会话”的意思，与 Cookie 类似，都是用来储存使用者相关资料的，比如用户名、访问权限、登陆时间等。与 Cookie 最大不同之处在于 Cookie 是将资料存放于客户端电脑之中，而 Session 则是将数据存放于服务器系统之下。
		+	当开启一个 Session 时，PHP 将会创建一个随机的 Session ID（例如“t5is1r7ct740dn390kuv3mpcse”），每个用户的 Session ID 都是唯一的，而且 Session ID 与服务器上存储该用户 Session 数据的文本文件名称相同。
		+	Session ID 会分别保存在客户端和服务器端两个位置。客户端，使用临时的 Cookie 保存在浏览器指定目录中，Cookie 名称默认为“PHPSESSID”；服务器端，以文本文件形式保存在指定的 Session 目录中。默认情况下，这个 Session ID 将作为一个 Cookie 发送给 Web 浏览器，接下来 PHP 页面将使用这个 Cookie 来访问 Session 的信息。
		+	与 Cookie 相比，Session 拥有以下的优势：通常情况下 Session 更加安全，因为 Session 中的数据不会在客户端和服务器端来回重复传递；Session 能够存储比 Cookie 更多的信息；在用户禁用 Cookie 的情况下，使用一些方法任然能保持 Session 正常工作。
	- 	Session 的工作原理
		+	我们可以使用 PHP 脚本创建和存储 Session 中的数据。在创建一个 Session 后，所有 Session 变量在用户一次会话期间里访问的所有页面都有效。
	- 	Session 的存储方式
		+	Session 默认会以文本的形式存储在服务器的临时目录中，文件名以“sess_”作为前缀，后面加上“Session ID”，例如“sess_t5is1r7ct740dn390kuv3mpcse”。
		+	我们可以在 php.ini 中找到 Session 的相关配置，下面是一些常用的配置信息：
		```
		session.save_handler = files                  #session 的存储方式，默认是文件，还可以是 redis 或者是 memcache
		session.save_path = "d:/wamp/tmp"    #session 文件的存储目录
		session.use_cookies = 1                        #是否使用 cookie 存储 session_id
		session.name = PHPSESSID                  #客户端存储 session_id 的会话名
		session.auto_start = 0                           #是否自动开启 session
		session.cookie_lifetime = 0                   #设置客户端中存储的 session_id 的过期时间，以秒为单位
		session.use_only_cookies=0                 #是否只使用 cookie 来处理 session_id
		session.gc_divisor = 1000                     #进程比率
		session.gc_probability = 1                    #垃圾回收的处理几率
		session.gc_maxlifetime = 1440             #设置 session 文件的过期时间
		```
	- 	Session 的生命周期
		+	Session 在以下情况会被删除，也就是失效：Session 超时，超时指的是连续一定时间服务器没有收到该 Session 所对应客户端的请求，并且这个时间超过了服务器设置的 Session 超时的最大时间；程序调用方法主动销毁 Session；服务器关闭或服务停止。
*	开启session
	- 	Session 的使用不同于 Cookie，在使用 Session 之前必须先启动，以便让 PHP 核心程序，将和 Session 相关的内建环境变量预先载入到内存中。
	- 	在php中可以使用session_start()来开启一个新的session会话
	```
	session_start([array $options = array()]);
	参数 $options（可选参数）为一个关联数组，如果提供该参数，那么会用其中的项目覆盖会话配置指示中的配置项。
	此数组中的键无需包含session.前缀。
	除了常规的会话配置指示项，还可以在此数组中包含 read_and_close 选项。
	如果将此选项的值设置为 TRUE，那么会话文件会在读取完毕之后马上关闭，因此，
	可以在会话数据没有变动的时候，避免不必要的文件锁。
	session_start() 会创建新会话或者重用现有会话。 如果通过 GET 或者 POST 方式，
	或者使用 cookie 提交了会话 ID，则会重用现有会话。
	session_start() 函数执行成功会开始会话并返回 TRUE，反之返回 FALSE。
	当会话自动开始或者通过 session_start() 手动开始的时候，PHP 内部会调用会话管理器的 open 和 read 回调函数。
	会话管理器可能是 PHP 默认的，也可能是扩展提供的（SQLite 或者 Memcached 扩展），
	也可能是通过 session_set_save_handler() 设定的用户自定义会话管理器。
	通过 read 回调函数返回的现有会话数据（使用特殊的序列化格式存储），PHP 会自动反序列化数据并且填充 $_SESSION 超级全局变量。
	```
	- 	调用 session_start() 函数会生成一个唯一的 Session ID，并保存在浏览器的 Cookie 中，默认名称为“PHPSESSID”。同时，在本地目录中生成一个以“sess_”加上 Session ID 组成的 Session 文件，用来存储 Session 中的数据
	-	删除SESSION的值
		+	当使用完一个 Session 变量后，可以将其删除；当完成一个会话后，也可以将其销毁。如果用户想退出 Web 系统，就需要为他提供一个注销的功能，把他的所有信息在服务器中销毁。
		+	使用unset()函数来删除指定session值。
		```
		unset(mixed $var[, mixed $...]);
		unset($_SESSION['the_key']);
		```
		+	删除整个的session值，可以将一个空数赋值给$\_SESSION数组。或者使用session_unset()函数。
		```
		$_SESSION = array();
		session_unset();
		```
		+	结束当前会话。如果整个 Session 会话结束，可以使用 session_destroy() 函数销毁当前会话的全部数据，即彻底销毁 Session，
		```
		session_destroy();
		session_destroy() 函数不需要传入任何参数，另外，session_destroy() 函数虽然可以销毁当前会话中的全部数据，但是不会重置 $_SESSION 数组，也不会重置 Cookie。如果需要再次使用 Session 会话，则必须重新调用 session_start() 函数。
		使用 $_SESSION = array() 清空 $_SESSION 数组的同时，也将这个用户在服务器端对应的 Session 文件内容清空。而使用 session_destroy() 函数时，则是将这个用户在服务器端对应的 Session 文件删除。
		```
*	session和cookie
	- 	Session 是用户进入某个网站到关闭浏览器这段时间的会话，默认以文件形式存在服务器磁盘中，所以设置过多的 Session 会影响磁盘的性能，也可以用 Memory 引擎存入 MySQL，因为内存引擎读写速度快，现在也可以指定用 Redis 来处理 Session，这样更快，效率更高。
	- 	通常 Cookie 与 Session 是绑定的，即用户在没有禁用 Cookie 时，Cookie 一般会保存 Session ID 及 Session 生存周期，如果用户删除 Cookie 一般会退出系统；如果没有禁用 Cookie 关闭浏览器 Session 也会立即失效，要重新登录系统。
	- 	Cookie 存储的数据在不同的浏览器会有不同的限制，一般在同一个域名下，Cookie 变量数量控制在 20 个以内，每个 Cookie 的值大小控制在 4kb 以内。Session 值没有大小和数量限制，但如果数量过多，会增大服务器的压力。另外，Cookie 保存的内容是字符串，而 Session 保存的数据是对象。
	- 	Cookie 与 Session 一般应于标识用户、权限认证、存储简单数据、还有就是利用 Cookie 实现单点登录。
	- 	Session 不能区分路径，同一个用户在访问一个网站期间，所有的 Session 在任何一个地方都可以访问到；而 Cookie 中如果设置了路径参数，那么同一个网站中不同路径下的 Cookie 是不能互相访问的。

### PHP错误和异常处理
*	PHP 的错误和异常处理是 PHP 中常用的模块之一，在开发项目的时候合理使用异常处理，将有利于发现错误并加快开发速度。
*	异常处理是一种可扩展、易维护的错误处理统一机制，并提供了一种新的面向对象的错误处理方式。
*	异常处理在 PHP 中的具体体现就是，PHP 提供了一个名叫 Exception 的类完成对 PHP 程序异常的处理，这个类包含了一些处理异常的函数，这些函数可以捕获程序异常和错误。
*	PHP中的异常处理类
	- 	PHP中提供了内置的异常处理类--Exception，该类中常用的成员函数为
		+	getMessage()：返回异常处理的内容。
		+	getCode()：以数字形式返回异常代码。
		+	getFile()：返回发生异常的文件名。
		+	getLine()：返回发生错误的代码行号。
		+	getTrace()：返回backtrace()数组。
		+	getTraceAsString()：返回已格式化成字符串的、由函数getTrace()产生的信息。
		+	\_\_toString()：产生异常的字符串信息，它可以重载。
	```
	Exception {
		protected string $message;
		protected int $code;
		protected string $file;
		protected int $line;

		public __construct ([ sting $message = '' [, int $code = 0 [, Throwable $previous = NULL ]]])
		final public getMessage ( void ) : string
		final public getPrevious ( void ) : Throwable
		final public getCode ( void ) : int
		final public getFile ( void ) : string
		final public getLine ( void ) : int
		final public getTrace ( void ) : array
		final public getTraceAsString ( void ) : string
		public __toString ( void ) : string
		final private __clone ( viod ) : void

	}
	```
*	捕获程序中的异常
	- 	在 PHP 中想要捕获程序中的异常，需要使用 try catch 语句和 throw 关键字来实现。try catch 语句和流程控制语句类似，所以可以通过 try catch 语句来实现一种另类的条件选择结构，而 throw 关键字则可以抛出一个异常。
	```
	try {
		// may could be error code
	} catch (Exception $e) {
		echo $e->getCode;
		echo $e->getMessage;
	}
	需要进行异常处理的代码都必须放入 try 代码块内，以便捕获可能存在的异常。每一个 try 至少要有一个与之对应的 catch。
	使用多个 catch 可以捕获不同的类所产生的异常。
	当 try 代码块不再抛出异常或者找不到 catch 能匹配所抛出的异常时，PHP 代码就会在跳转到最后一个 catch 的后面继续执行。
	在 PHP 代码中所产生的异常可以被 throw 语句抛出并被 catch 语句捕获。当然，PHP 允许在 catch 代码块内再次抛出（throw）异常。
	当一个异常被抛出时，其后的代码不会再继续执行，而 PHP 就会尝试继续查找第一个能与之匹配的 catch。
	如果一个异常没有被捕获，而且又没用使用 set_exception_handler() 作相应的处理的话，将会产生一个严重的错误，
	并且输出 UncaughtException...（未捕获异常）的提示信息。
	```
	- 	创建自己的异常类
		+	在各种语言里，对异常和错误的定义不同。在 PHP 里遇到任何错误都会抛出一个错误，很少会主动抛出异常，不像 Java 语言那样会预先定义好各种异常类，当程序执行到异常处的代码时会主动抛出。
		+	PHP 的异常处理机制并不完善，在 PHP 中想处理不可预料的异常是办不到的，必须事先定义一些异常，将各种可能出现的异常进行 if…else 判断，手动抛出异常，所以在 PHP 里经常会使用到我们自己创建的异常类。
*	PHP的错误类型
	- 	任何程序员在开发时都可能会或多或少的造成一些失误，或其他原因造成错误的发生。当然，如果用户不愿意或不遵循应用程序的约束，也可能会在使用时引起一些错误发生。
	- 	PHP 程序的错误发生一般分为三类，分别是语法错误、执行时错误和逻辑错误。
		+ 	语法错误。语法错误是在编程中最容易碰到也是最容易解决的一种错误，例如：遗漏一个分号时就会显示错误信息。这种错误会停止程序的执行，并显示出错信息。我们可以根据错误信息改正程序，然后重新执行。
		+ 	运行时错误。运行时错误也是就在程序执行时出现的错误。这种程序的语法没有错误，但是在执行的过程中，PHP 会发现程序有些不合理的地方，从而提示出警告信息，但程序会继续向下执行。
		+ 	逻辑错误。逻辑错误是一种发生在程序员思想上的错误。在发生逻辑错误时并没有明显的错误信息，因为程序在执行中不会报出任何的错误信息，并且程序会正常执行，只是输出的结果并不是我们期望的而已。
*	PHP中的错误等级
	- 	PHP 中定义了许多不同级别的错误，如使用了未定义的变量会报出一个 notice 级别的错误，实例化一个未定义的类则会报出 fatal error 级别的错误。
	- 	可在 php.ini 配置文件中定义错误级别，如error_reporting=E_ALL|E_STRICT会设置最严格的错误级别，在代码中也可使用error_reporting(E_ALL)等来定义错误级别。
	- 	在正式环境中，可能会发生各种未知的错误，这时可以定义 error_reporting(0)，这样就能屏蔽错误了，用户不会在页面看到错误信息，而当排查错误时，依然可到 PHP 的执行错误日志中寻找相关信息。

| 值     | 常量                    | 说明                                                                                             |
|:-----:|:---------------------:|:----------------------------------------------------------------------------------------------:|
| 1     | E\_ERROR              | 致命的运行时错误，一般是不可恢复的情况，例如内存分配导致的问题，后果是导致脚本终止、不再继续运行。                                              |
| 2     | E\_WARNING            | 运行时警告（非致命错误），仅给出提示信息，但是脚本不会终止运行。                                                               |
| 4     | E\_PARSE              | 编译时语法解析错误，仅由分析器产生。                                                                             |
| 8     | E\_NOTICE             | 运行时通知，表示脚本遇到可能会表现为错误的情况，但是在可以正常运行的脚本里面也可能会有类似的通知。                                              |
| 16    | E\_CORE\_ERROR        | 在 PHP 初始化启动过程中发生的致命错误，类似 E\_ERROR，但是是由 PHP 引擎核心产生的。                                            |
| 32    | E\_CORE\_WARNING      | PHP 初始化启动过程中发生的警告（非致命错误），类似 E\_WARNING ，但是是由 PHP 引擎核心产生的。                                      |
| 64    | E\_COMPILE\_ERROR     | 致命编译时错误，类似 E\_ERROR，但是是由 Zend 脚本引擎产生的。                                                         |
| 128   | E\_COMPILE\_WARNING   | 编译时警告（非致命错误），类似 E\_WARNING，但是是由 Zend 脚本引擎产生的。                                                  |
| 256   | E\_USER\_ERROR        | 用户产生的错误信息，类似 E\_ERROR，但是是由用户自己在代码中使用 PHP 函数 trigger\_error\(\) 来产生的。                           |
| 512   | E\_USER\_WARNING      | 用户产生的警告信息，类似 E\_WARNING，但是是由用户自己在代码中使用 PHP 函数 trigger\_error\(\) 来产生的。                         |
| 1024  | E\_USER\_NOTICE       | 用户产生的通知信息，类似 E\_NOTICE，但是是由用户自己在代码中使用 PHP 函数 trigger\_error\(\) 来产生的。                          |
| 1024  | E\_STRICT             | 启用 PHP 对代码的修改建议，以确保代码具有最佳的互操作性和向前兼容性。                                                          |
| 2048  | E\_RECOVERABLE\_ERROR | 可被捕捉的致命错误，表示发生了一个可能非常危险的错误，但是还没有导致 PHP 引擎处于不稳定的状态。如果该错误没有被用户自定义句柄捕获，将成为一个 E\_ERROR，从而使脚本终止运行。  |
| 8192  | E\_DEPRECATED         | 运行时通知，启用后将会对在未来版本中可能无法正常工作的代码给出警告。                                                             |
| 16384 | E\_USER\_DEPRECATED   | 用户产生的警告信息，类似 E\_DEPRECATED，但是是由用户自己在代码中使用 PHP 函数 trigger\_error\(\) 来产生的。                      |
| 30719 | E\_ALL                | E\_STRICT 除外的所有错误和警告信息。                                                                        |

*	PHP中的错误日志配置和使用
	-	对于 PHP 开发者来说，一旦某个项目投入使用，应该立即将配置文件 php.ini 中的 display_errors 选项关闭，以免因为这些错误所透露的路径、数据库连接、数据表等信息而遭到黑客攻击。
	-	我们可以在单独的文本文件中将错误报告作为日志记录。错误日志的记录，可以帮助开发人员或者管理人员查看系统是否存在问题。如果需要将程序中的错误报告写入错误日志中，只要在 PHP 的配置文件中，将配置项 log_errors 开启即可。
	-	错误报告默认会记录到 Web 服务器的日志文件里，例如记录到 Apache 服务器的错误日志文件 error.log 中。当然也可以将错误日志记录到指定的文件中或发送到系统的 syslog（系统日志）中
	-	使用指定的文件记录错误报告日志
		+	如果想使用自己指定的文件记录错误日志，一定要确保这个文件存放在文档根目录之外，以减少遭到攻击的可能。并且该文件一定要让 PHP 脚本具有写权限。假设在 Linux 操作系统中，将 /usr/local/ 目录下的 error.log 文件作为错误日志文件，并设置 Web 服务器进程用户具有写的权限。然后在 PHP 的配置文件中，将 error_log 指令的值设置为这个错误日志文件的绝对路径。
		```
		// 需将php.ini进行如下配置
		error_reporting = E_ALL							// 将会向PHP报告发生的每个错误
		display_errors = Off								// 不显示满足上条指令所定义规则的所有错误报告
		log_errors = On										// 决定日志语句记录的位置
		log_errors_max_len = 1024						// 设置每个日志项的最大长度
		error_log = E:/php_log/php_error.log		// 指定产生的错误日志写入的日志文件位置
		```
		+	PHP 的配置文件按上面的方式设置完成以后，并重新启动 Web 服务器。这样，在执行 PHP 的任何脚本文件时，产生的所有错误报告都不会在浏览器中显示，而会记录在自己指定的错误日志 E:/php_log/php_error.log 中。
		+	仅可以记录满足 error_reporting 所定义规则的所有错误，而且还可以使用 PHP 中的 error_log() 函数把错误信息发送到 web 服务器的错误日志或者到一个文件里。
		```
		error_log(string $message [, int $type = 0 [, string $destination [, string $extra_headers ]]]) : bool;
		参数说明如下：
		$message：需要记录的错误信息；
		$message_type：设置错误应该发送到何处。可能的信息类型有以下几个：
			0：（默认值）将 $message 发送到 PHP 的系统日志，使用操作系统的日志机制或者一个文件，取决于配置文件中 error_log 设置了什么；
			1：将 $message 发送到参数 $destination 设置的邮件地址。 第四个参数 $extra_headers 只有在这个类型里才会被用到；
			2：（已废弃）不再是一个选项；
			3：$message 被发送到位置为 $destination 的文件里。字符 $message 不会默认被当做新的一行；
			4：将 $message 直接发送到 SAPI 的日志处理程序中。
		$destination：目标，也就是错误消息被发送到的目的地。它的含义描述于以上，由 $message_type 参数所决定；
		$extra_headers：额外的头。当 $message_type 设置为 1 的时候使用。 该信息类型使用了 mail() 的同一个内置函数。
		```
	-	错误信息记录到操作系统的日志里
		+	错误报告也可以被记录到操作系统日志里，但不同的操作系统之间的日志管理也是不同的。在 Linux 上错误语句将送往 syslog（基于UNIX的日志工具），而在 Windows 上错误将发送到事件日志里，可以通过事件查看器来查看。
		+	如果希望将错误报告写到操作系统的日志里，将 php.ini 配置文件中 error_log 项的值设置为 syslog 即可。
		```
		error_reporting = E_ALL							// 将会向PHP报告发生的每个错误
		display_errors = Off								// 不显示满足上条指令所定义规则的所有错误报告
		log_errors = On										// 决定日志语句记录的位置
		log_errors_max_len = 1024						// 设置每个日志项的最大长度
		error_log = syslog		// 指定产生的错误日志写入的日志文件位置
		```
		+	除了一般的错误输出之外，PHP 还允许向系统 syslog 中发送定制的消息。虽然通过前面介绍的 error_log() 函数，也可以向 syslog 中发送定制的消息，但在 PHP 中为这个特性提供了需要一起使用的 3 个专用函数，
			1.	openlog()，打开一个当前系统中日志器的连接，为向系统插入日志消息做好准备。并将提供的第一个字符串参数插入到每个日志消息中，该函数还需要指定两个将在日志上下文使用的参数，可以参考官方文档使用。
			2.	syslog()，该函数向系统日志中发送一个定制消息。需要两个必选参数，第一个参数通过指定一个常量定制消息的优先级。例如 LOG_WARNING 表示一般的警告；LOG_EMERG 表示严重地可以预示着系统崩溃的问题，一些其他的表示严重程度的常量可以参考官方文档使用。第二个参数则是向系统日志中发送的定制消息，需要提供一个消息字符串，也可以是 PHP 引擎在运行时提供的错误字符串。
			3.	closelog()，该函数在向系统日志中发送完成定制消息以后调用，用来关闭由 openlog() 函数打开的日志连接。
	- 	set_error_handler()，自定义错误处理函数
		+	php提供一个php_error_handler()函数，该函数可以指定另外一个函数作为错误处理函数。
		```
		set_error_handler(callable $error_handler [, int $error_types = E_ALL | E_STRICT]);
		1) $error_handler 是用户自定义的函数名称，除了可以传入函数名，还可以传入 NULL 重置处理程序到默认状态，还可以传入引用对象和对象方法名的数组。
		error_handler(int $errno, string $errstr [, string $errfile [, int $errline [, array $errcontext]]]);
		第一个参数 $errno 表示错误的级别，是一个 integer 类型；
		第二个参数 $errstr 表示错误的信息，是一个 string 类型；
		第三个参数 $errfile 是一个可选参数，表示发生错误的文件名，是一个 string 类型；
		第四个参数 $errline 也是一个可选参数，表示发生错误的行号，是一个 integer 类型；
		第五个参数 $errcontext 同样是一个可选参数（在 PHP7.2.0 之后被弃用），表示错误发生时活动符号表的 array。也就是说 $errcontext 会包含错误触发处作用域内所有变量的数组。用户的错误处理程序不应该修改错误上下文（context）。
		如果 set_error_handler() 函数返回 FALSE，标准错误处理程序将会继续调用。
		2) $error_types 参数就像配置文件 php.ini 中 error_reporting 能够控制错误的显示一样，此参数能够用于屏蔽 $error_handler 的触发。如果没有该掩码，无论 $error_reporting 是如何设置的，$error_handler 都会在每个错误发生时被调用。
		如果之前有定义过错误处理程序，则返回该程序名称；如果是内置的错误处理程序，则返回 NULL。如果指定了一个无效的回调函数，同样会返回 NULL。如果之前的错误处理程序是一个类的方法，此函数会返回一个带类和方法名的索引数组（indexed array）。
		```
	-	PHP屏蔽错误报告
		+	在表达式前使用@符号，可以屏蔽单行php语句错误显示。
		```
		$link = @mysqli_connect('127.0.0.1', 'user', 'user', 'mydbName') or die('connecting fairl');
		```
		+	使用php_reporting(0)函数在php单个文件中屏蔽错误显示。
		```
		php_reporting(int $level);
		其中参数 $level 为设置错误级别，如果将 $level 设置为 0，将关闭所有 PHP 错误报告；如果设置为 -1，将返回所有的错误报告。
		```
		+	修改php配置文件参数error_displays的值为Off，可关闭所有的php错误报告。

### php与mysql数据库
*	php连接数据库
	-	使用 PHP 操作 MySQL 数据库是进行 Web 开发的必然要求之一，PHP 中提供了完整的操作 MySQL 数据库的函数，这些函数包括了从连接数据库、执行 SQL 语句、处理数据结果集到关闭数据库的方方面面。
	-	php访问数据库的步骤，建立与数据库的连接、选择要使用的数据库、创建SQL语句、执行SQL语句、获取SQL语句执行的结果、处理数据结果集和关闭与数据库的连接。
	-	在此之前，我们需要确保开启了 PHP 中的 mysqli 扩展。以 Windows 系统为例，开启 mysqli 扩展就是将 php.ini 配置文件中extension=mysqli（php7）或extension=php_mysqli.dll（php5）一项的注释去掉即可。
	-	连接mysql数据库
		+	开启了mysqli扩展之后，使用mysqli_connect()函数连接mysql数据库
		```
		mysqli_connect(
			[string $host = ini_get('mysqli.default_host')
			[, string $username = ini_get('mysqli.default_user')
			[, string $password = ini_get('mysqli.default_pw')
			[, string $dbname = ''
			[, int $port = ini_get('mysqli.default_port')
			[, string $socket = ini_get('mysql.default_socket')
		]]]]]]);
		参数说明如下：
		$host：可选参数，要连接的服务器。可以是主机名或者是 IP 地址；
		$username：可选参数，登录所使用的 MySQL 用户名；
		$password：可选参数，登录所用的密码；
		$dbname：可选参数，执行查询时使用的默认数据库；
		$port：可选参数，指定连接到 MySQL 服务器的端口号；
		$socket：可选参数，指定 socket 或要使用的已命名 pipe，这个参数在开发中很少用到。
		另外需要注意的是，mysqli_connect() 函数是 mysqli::__construct() 函数的别名，所有使用对象 mysqli() 也可以实现连接数据库。
		```
	-	选择数据库
		+	mysqli_connect()函数中dbname参数是可以省略的，当数据库连接成功之后，我们可以使用mysqli_select_db()函数来选择数据库
		```
		// 面向对象的风格写法
		mysqli::select_db(sting $dbname);
		$dbname为指定的数据库名称
		// 面向过程的风格写法
		mysqli_select_db(mysqli $mysql, string $dbname);
		其中，$link 为通过 mysqli_connect() 函数返回的数据库连接，$dbname 为指定的数据库名称。
		函数执行成功会返回 TRUE，执行失败则返回 FALSE。
		```
		+	通常我们可以将数据库连接和选择默认数据库的过程写在一个单独的 PHP 脚本文件中。在其他需要对数据库操作的 PHP 脚本中，只要使用 require() 或 include() 等函数将该文件包含进来即可，不需要再重复编写连接数据库的代码了。这样做不仅可以提高开发效率，更重要的是当数据库的用户名和密码需要变化时，只需要更改这一个文件即可，所有使用该文件的 PHP 脚本都是使用新用户与数据库服务器建立的连接。
	-	执行SQL语句
		+	使用mysqli_query()函数来执行SQL语句。成功选择好 MySQL 数据库后，接下来就可以对所选数据库中的数据表进行查询、更改以及删除等操作
		```
		// 面向对象的风格写法
		mysqli::query(string $query [, int $resultmode = MYSQLI_STORE_RESULT]);
		参数说明：
		$query：要执行的 SQL 语句；
		$resultmode：可选参数，用来修改函数的行为。可以是下列值的任意一个：
		MYSQLI_USE_RESULT（如果需要查询大量数据，使用这个）；
		MYSQLI_STORE_RESULT（默认值）。
		// 面向过程风格写法
		mysqli_query(mysqli $link, string $query [, int $resultmode = MYSQLI_STORE_RESULT]); 
		参数说明：
		$query：要执行的 SQL 语句；
		$resultmode：可选参数，用来修改函数的行为。可以是下列值的任意一个：
		MYSQLI_USE_RESULT（如果需要查询大量数据，使用这个）；
		MYSQLI_STORE_RESULT（默认值）。
		提示：函数执行失败时会返回 FALSE；而通过 mysqli_query() 成功执行 SELECT、SHOW、DESCRIBE 或 EXPLAIN 查询时则会返回一个 mysqli_result 对象；其他查询执行成功则返回 TRUE。
		```
	-	获取SQL查询结果
		+	mysqli_fetch_row(), 从结果集中取得一行，并以索引数组的形式返回；
		+	mysqli_fetch_assoc(), 从结果集中取得一行，并以关联数组的形式返回；
		+	mysqli_fetch_array(), 从结果集中取得一行，并以关联数组、索引数组或二者兼有的形式返回；
		+	mysqli_fetch_all(), 从结果集中取得所有行，并以关联数组、索引数组或二者兼有的形式返回；
		```
		mysqli_result::fetch_array([int $resulttype = MYSQLI_BOTH])
		其中 mysqli_result 为使用 mysqli_query() 函数获取的结果集；$resulttype 为可选参数，它是一个常量，用来设定返回值的类型，它的取值可以是 MYSQLI_ASSOC、MYSQLI_NUM 或 MYSQLI_BOTH。
		```
		+	mysqli_fetch_object(), 从结果集中取得一行，并以对象的形式返回。
		```
		mysqli_result::fetch_object([string $class_name = "stdClass"[, array $params]])
		其中 mysqli_result 为使用 mysqli_query() 函数获取的结果集；$class_name 为可选参数，用来规定实例化的类名称，设置属性并返回；$params 为可选参数，用来规定一个传给$classname 的构造函数的可选参数数组。
		```	
	-	获取SQL查询结果的行数
		+	想要获取由 SELECT 语句查询到的结果集中有多少条数据的话，则需要使用 mysqli_num_rows()，
		```
		$mysqli_result->num_rows;
		mysqli_num_rows(mysqli_result $result);
		mysqli_num_rows() 函数仅对 SELECT 语句有效，如果返回的行数大于 PHP_INI_MAX，则将行数以字符串的形式返回。
		```
	-	一次执行多条SQL语句
		+	如果需要一次执行多条 SQL 命令，就必须使用 PHP 中的 mysqli_multi_query() 函数
		```
		mysqli::multi_query(string $query);
		mysqli_multi_query($link, $query);
		参数 $query 中可以包含多条 SQL 命令，每条 SQL 命令之间使用分号;分隔。如果第一条 SQL 命令在执行时没有出错，那么这个函数就会返回 TRUE，否则将返回 FALSE。因为 mysqli_multi_query() 函数能够连接执行一个或多个查询，而每条 SQL 命令都可能返回一个结果，在必要时需要获取每一个结果集。所以对该函数返回结果的处理也有了一些变化，第一条查询命令的结果要用 mysqli_use_result() 或 mysqli_store_result() 函数来读取。
		也可以使用 mysqli_store_result() 函数将全部结果立刻取回到客户端，而且这么做的效率更高。另外，可以使用 mysqli_more_results() 函数检查是否还有其他结果集。如果想对下一个结果集进行处理，可以使用 mysqli_next_result() 函数获取下一个结果集，有下一个结果集时该函数返回 TRUE，没有时返回 FALSE，在有下一个结果集的情况下，也需要使用  mysqli_use_result() 或 mysqli_store_result() 函数来读取结果集的内容。
		```
	-	PHP PDO类
		+	mysqli 只能支持 MySQL 数据库。如果我们要连接其它的数据库要用PDO 类。
		+	PDO 是 PHP Date Object（PHP 数据对象）的简称，它是 PHP 为访问数据库定义的一个轻量级的、一致性的接口，它提供了一个数据访问抽象层，这样无论你使用什么数据库，都可以通过同一函数执行查询和获取数据，大大简化了数据库的操作，并能够屏蔽不同数据库之间的差异。有了 PDO 就不必再使用 mysqli_* 的一系列函数了，只需要使用 PDO 中的方法就可以对数据库进行操作。
		+	PDO 的特点
			1.	我们可以将 PDO 看作是一个“数据库访问抽象层”，作用是统一各种数据库的访问接口。与 MySQL 和 MSSQL 函数库相比，PDO 让跨数据库的使用更具有亲和力，与 ADODB 和 MDB2 相比，PDO 更加高效。
			2.	PDO 将通过一种轻型、清晰、方便的函数，统一各种不同的数据库的共有特性，实现 PHP 脚本在最大程度上的抽象性和兼容性。
			3.	PDO 吸取了现有数据库扩展成功和失败的经验教训，利用 PHP5 的最新特性，可以轻松地与各种数据库进行交互。
			4.	PDO 扩展是模块化的，能够在运行时为用户数据库后端加载驱动程序，而不必重新编译或重新安装整个 PHP 程序。例如，PDO_MySQL 扩展会替代 PDO 扩展实现 MySQL 数据库 API，它还有一些用于 Oracle、Postgre SQL、ODBC 和 Firebird 的驱动程序。
		+	开启PDO，默认情况下，PDO 在 PHP 中为开启状态，但是要启用对某个数据库驱动程序的支持，仍需要进行相应的配置操作。可在配置文件中，通过去掉对应扩展名的注释来开启对应的扩展
		```
		;extension=pdo_firebird				// firenbirdDB
		;extension=php_pdo_mysql.dll		// MySQLDB
		;extension=pdo_oci						// OracleDB
		;extension=pdo_odbc					// OBDC
		;extension=pdo_pgsql					// Postgre SQL
		;extension=pdo_sqlite					// SQLite
		```
	-	创建PDO对象
		+	在使用 PDO 与不同数据库之间交互时，PDO 对象中的成员方法是统一各种数据库的访问接口，所以在使用 PDO 与数据库交互之前，首先要创建一个 PDO 对象，然后再通过对象的构造函数来连接数据库。
		```
		PDO::__construct(string $dsn[, string $username[, string $password[, array $driver_options]]]);
		参数说明如下：
		$dsn：数据源名称或叫做 DSN（Data Source Name 的缩写），包含了请求连接到数据库的信息。通常一个 DSN 是由 PDO 驱动程序的名称，紧随其后是一个冒号，再后面是可选的驱动程序的数据库连接信息，比如主机名、端口和数据库名。以 MySQL 数据库为例 $dsn 可以定义为：mysql:host=localhost;port=3306;dbname=dbname;charset=utf8，分别定义了数据库类型、端口号、数据库名和字符集；
		$username：可选参数，用来表示 DSN 字符串中的用户名；
		$password：可选参数，用来表示 DSN 字符串中的密码；
		$driver_options：可选参数，一个具体驱动的连接选项的键/值数组
		```
	-	PDO执行SQL语句
		+	在 PDO 中，我们可以使用三种方式来执行 SQL 语句，分别是 exec() 方法，query() 方法，以及预处理语句 prepare() 和 execute() 方法。
		+	exec()方法，当执行INSERT, UPDATE, DELECT等不需要返回结果集的SQL语句时，可使用PDO中的exec()方法。
		```
		PDO::exec(string $sql);
		exec() 方法不会从 SELECT 查询语句中获取相应的结果。
		```
		+	query()方法，当执行需要返回结果集的SELECT查询语句时，可以使用PDO中的query()方法。如果该方法执行成功，则会返回PDOstatement对象。如果想获取数据行的总数可以，使用PDOstatement对象中的rowCount()方法获取。
		```
		PDO::query(string $sql);
		PDO::query(string $sql, int $PDO::FETCH_COLUMN, int $colno);
		PDO::query(string $sql, int $PDO::FETCH_CLASS, string $classname, array $clotatgs);
		PDO::query(string $sql, int $PDO::FETCH_INTO, object $object);
		其中 $sql 为要执行的 SQL 语句；其余的参数用来为语句设置默认的获取模式，相当于调用结果对象 PDOStatement::setFetchMode() 方法。

		query() 和 exec() 都可以执行所有的 SQL 语句，只是返回值不同而已；
		query() 可以实现所有 exec() 的功能；
		当把 select 语句应用到 exec() 时，总是返回 0；
		如果要看查询的具体结果，可以通过 foreach 语句完成循环输出。
		```
		+	perpare()和execute()方法，当同一个查询需要多次执行时（有时需要迭代传入不同的参数），使用预处理语句的方式来实现效率会更高。使用预处理语句就需要使用 PDO 对象中的 prepare() 方法去准备一个将要执行的查询，再使用 PDOStatement 对象中的 execute() 方法来执行。
		```
		PDO::prepare(string $statement[,  array $driver_options = array()]);
		参数说明如下：
		$statement：必须是对目标数据库有效的 SQL 语句模板；
		$driver_options：为可选参数（数组类型），包含一个或多个键值对，为返回的 PDOStatement 对象设置属性。最常使用到是将 PDO::ATTR_CURSOR 值设置为 PDO::CURSOR_SCROLL 来请求一个可滚动游标。
		SQL 语句模板中可以包含零个或多个参数占位标记，格式可以是命名（:name）或问号（?）的形式，当它执行时将用真实数据取代。在同一个 SQL 语句里，命名和问号形式不能同时使用，只能选择其中一种参数形式。如果使用命名形式的占位标记，那么标记的命名必须是唯一的。
		PDOStatement::execute([array $input_parameters]);
		参数 $input_parameters 为一个元素个数和将被执行的 SQL 语句中绑定的参数一样多的数组。绑定的值不能超过指定的个数，如果在 $input_parameters 中存在比 PDO::prepare() 预处理的 SQL 指定的多的键名，则此语句将会失败并发出一个错误。
		```
	-	PDO中获取结果集
		+	PDO 的数据获取方法与其他数据库扩展都非常类似，只要成功执行 SELECT 查询，都会有结果集对象生成。不管是使用 PDO 对象中的 query() 方法，还是使用 prepare() 和 execute() 等方法结合的预处理语句，执行 SELECT 查询都会得到结果集对象 PDOStatement。
		+	fetch(), 可以从一个 PDOStatement 对象的结果集中获取当前行的内容，并将结果集指针移至下一行，当到达结果集末尾时返回 FALSE，
		```
		PDOStatement::fetch([int $fetch_style[, int $cursor_orientation = PDO::FETCH_ORI_NEXT[, int $cursor_offset = 0]]]);
		参数说明如下：
		$fetch_style：可选参数，用来控制下一行如何返回给调用者。此值必须是 PDO::FETCH_* 系列常量中的一个，如下所示：
		PDO::FETCH_ASSOC：返回一个关联数组；
		PDO::FETCH_BOTH（默认）：返回一个索引数组加关联数组混合的数组；
		PDO::FETCH_BOUND：返回 TRUE，并分配结果集中的值给 PDOStatement::bindColumn() 方法绑定的 PHP 变量；
		PDO::FETCH_CLASS：返回一个请求类的新实例，映射结果集中的列名到类中对应的属性名。如果 fetch_style 包含 PDO::FETCH_CLASSTYPE（例如：PDO::FETCH_CLASS | PDO::FETCH_CLASSTYPE），则类名由第一列的值决定；
		PDO::FETCH_INTO：更新一个被请求类已存在的实例，映射结果集中的列到类中命名的属性；
		PDO::FETCH_LAZY：结合使用 PDO::FETCH_BOTH 和 PDO::FETCH_OBJ，创建供用来访问的对象变量名；
		PDO::FETCH_NUM：返回一个索引数组；
		PDO::FETCH_OBJ：返回一个属性名对应结果集列名的匿名对象。
		$cursor orientation：可选参数，用来确定当对象是一个可滚动的游标时应当获取哪一行。此值必须是 PDO::FETCH_ORI_* 系列常量中的一个，默认为 PDO::FETCH_ORI_NEXT。要想让 PDOStatement 对象使用可滚动游标，必须在用 PDO::prepare() 预处理 SQL 语句时，设置 PDO::ATTR_CURSOR 属性为 PDO::CURSOR_SCROLL；
		$offset：可选参数，当参数 $cursor_orientation 设置为 PDO::FETCH_ORI_ABS 时，此值指定结果集中想要获取行的绝对行号；当参数 $cursor_orientation 设置为 PDO::FETCH_ORI_REL 时，此值指定想要获取行相对于调用 PDOStatement::fetch() 前游标的位置。
		```
		+	fetchAll(), 该方法只需要调用一次就可以获取结果集中的所有行，并赋给返回的数组。
		```
		PDOStatement::fetchAll([int $fetch_style[, mixed $fetch_argument[, array $ctor_args = array()]]])
		参数说明如下：
		$fetch_style：可选参数，用来控制返回数组的内容，默认值为 PDO::FETCH_BOTH。该参数的取值与 fetch() 方法相同；
		$fetch_argument：根据 $fetch_style 参数的值，此参数有不同的意义：
		PDO::FETCH_COLUMN：返回指定以 0 开始索引的列；
		PDO::FETCH_CLASS：返回指定类的实例，映射每行的列到类中对应的属性名；
		PDO::FETCH_FUNC：将每行的列作为参数传递给指定的函数，并返回调用函数后的结果。
		$ctor_args：当 $fetch_style 参数为 PDO::FETCH_CLASS 时，自定义类的构造函数的参数。
		```
		+	fetchColumn(), 获取结果集中当前行指定字段的值，
		```
		PDOStatement::fetchColumn([int $column_number = 0])
		其中参数 $column_number 为想从行里取回的列的索引数字（以 0 开始）。如果没有提供值，则获取第一列。
		```
	-	PDO中的错误处理
		+	在 PDO 中有两个获取程序中错误信息的方法，分别是 errorCode() 方法和 errorInfo() 方法。
		+	PDO 中一共提供了三种不同的错误处理模式，不仅可以满足不同风格的编程，也可以调整扩展处理错误的方式。
			1.	PDO::ERRMODE_SILENT，PDO::ERRMODE_SILENT 为默认模式。 在发生错误时 PDO 将只简单地设置错误码，不做其它任何操作。可使用 PDO::errorCode() 和 PDO::errorInfo() 两个方法来检查语句和数据库对象。如果错误是由于调用语句对象而产生的，那么可以调用这个对象的 PDOStatement::errorCode() 或 PDOStatement::errorInfo() 方法。如果错误是由于调用数据库对象而产生的，那么可以在数据库对象上调用这两个方法
			2.	PDO::ERRMODE_WARNING，PDO::ERRMODE_WARNING 模式除了会设置错误码之外，PDO 还将发出一条传统的 E_WARNING 信息。在调试/测试期间如果只是想看看发生了什么问题，而不中断程序运行的话，可以使用该模式。
			3.	PDO::ERRMODE_EXCEPTION，PDO::ERRMODE_EXCEPTION 模式除了会设置错误码之外，PDO 还将抛出一个 PDOException 异常类并设置它的属性来反射错误码和错误信息。此模式在调试期间也非常有用，因为它会有效地放大脚本中产生错误的点，从而可以非常快速地指出代码中有问题的潜在区域。
		+	PDO 使用 SQL-92 SQLSTATE 来规范错误码字符串，不同 PDO 驱动程序负责将它们的本地代码映射为适当的 SQLSTATE 代码。PDO::errorCode() 方法返回一个单独的 SQLSTATE 码。如果需要更多这个错误的细节信息，PDO 还提供了一个 PDO::errorInfo() 方法来返回一个包含 SQLSTATE 码、特定驱动错误码以及此驱动的错误字符串的数组。
		+	异常模式另一个非常有用的作用是，相比传统 PHP 风格的警告，可以更清晰地构建自己的错误处理，而且比起静默模式和显式地检查每种数据库调用的返回值，异常模式需要的代码/嵌套更少。
		```
		try {
		    $pdo = new PDO($dsn, $user, $pwd);
		    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
		} catch (Exception $e) {
		    die('Can not connect: '.$e->getMessage());
		}
		// 使用 PDO::setAttribute() 方法设置错误模式外，还可以在创建 PDO 实例时设置错误模式
		$pdo = new PDO($dsn, $user, $pwd, array(PDO::ATTR_ERRMODE => PDO::ERRMODE_WARNING));
		```
		+	PDO::errorCode()，用于获取在操作数据库句柄时所发生的错误代码，这些错误代码被称为 SQLSTATE 代码。PDO::errorCode() 方法可以返回一个 SQLSTATE，一个由 5 个字母或数字组成的在 ANSI SQL 标准中定义的标识符。 简要地说，一个 SQLSTATE 由前面两个字符的类值和后面三个字符的子类值组成。
		+	PDO::errorInfo()，用于获取操作数据库句柄时所发生的错误信息。errorInfo() 方法的返回值为一个数组，它包含了相关的错误信息。
		+	PDO 和 PDOStatement 对象中都有 errorCode() 和 errorInfo() 方法，如果没有任何错误，errorCode() 返回的是 00000；否则就会返回一些错误代码。而 errorInfo() 返回的是一个数组，包括 PHP 定义的错误代码和 MySQL 的错误代码及错误信息。
	-	设置与获取PDO属性
		+	在 PDO 对象中有很多属性可以用来调整 PDO 的行为或获取底层驱动程序状态，如果在创建 PDO 对象时，没有在构造方法中最后一个参数设置过的属性选项，可以在对象创建完成以后，通过 PDO 对象中的 setAttribute() 和 getAttribute() 方法设置和获取这些属性的值。
		+	PDO::getAttribute()，需要提供一个参数，传递一个特定的属性名称，执行成功后会返回该属性所指定的值，否则返回 NULL，
		```
		PDO::getAttribute(int $attribute)
		其中参数 $attribute 为 PDO::ATTR_* 常量中的一个，下列为应用到数据库连接中的常量：
		PDO::ATTR_AUTOCOMMIT
		PDO::ATTR_CASE
		PDO::ATTR_CLIENT_VERSION
		PDO::ATTR_CONNECTION_STATUS
		PDO::ATTR_DRIVER_NAME
		PDO::ATTR_ERRMODE
		PDO::ATTR_ORACLE_NULLS
		PDO::ATTR_PERSISTENT
		PDO::ATTR_PREFETCH
		PDO::ATTR_SERVER_INFO
		PDO::ATTR_SERVER_VERSION
		PDO::ATTR_TIMEOUT
		```
		+	PDO::setAttribute()，用来设置数据库句柄的属性，
		```
		PDO::setAttribute(int $attribute, mixed $value)
		这个方法需要两个参数，第一个参数 $attribute 提供 PDO 对象特定的属性名，第二个参数 $value 则是为这个指定的属性赋一个值。下面列出了一些可用的通用属性名称和可以使用的值：
		PDO::ATTR_CASE：强制列名为指定的大小写；
		PDO::CASE_LOWER：强制列名小写；
		PDO::CASE_NATURAL：保留数据库驱动返回的列名；
		PDO::CASE_UPPER：强制列名大写。
		PDO::ATTR_ERRMODE：错误报告；
		PDO::ERRMODE_SILENT：仅设置错误代码；
		PDO::ERRMODE_WARNING：引发 E_WARNING 错误；
		PDO::ERRMODE_EXCEPTION：抛出 exceptions 异常。
		PDO::ATTR_ORACLE_NULLS：（在所有驱动中都可用，不仅限于Oracle）转换 NULL 和空字符串；
		PDO::NULL_NATURAL：不转换；
		PDO::NULL_EMPTY_STRING：将空字符串转换成 NULL；
		PDO::NULL_TO_STRING：将 NULL 转换成空字符串。
		PDO::ATTR_STRINGIFY_FETCHES：提取的时候将数值转换为字符串；
		PDO::ATTR_STATEMENT_CLASS：设置从 PDOStatement 派生的用户提供的语句类。不能用于持久的 PDO 实例。需要 array(string 类名, array(mixed 构造函数的参数))；
		PDO::ATTR_TIMEOUT：指定超时的秒数。不同驱动之间可能会有差异，比如 SQLite 等待的时间达到此值后就会放弃获取可写锁，但其他驱动可能会将此值解释为一个连接或读取超时的间隔；
		PDO::ATTR_AUTOCOMMIT：（在 OCI，Firebird 以及 MySQL 中可用）是否自动提交每个单独的语句；
		PDO::ATTR_EMULATE_PREPARES：启用或禁用预处理语句的模拟。有些驱动不支持或有限度地支持本地预处理，使用此设置可以强制 PDO 总是模拟预处理语句，或试着使用本地预处理语句。如果驱动不能成功预处理当前查询，它将总是回到模拟预处理语句上；
		PDO::MYSQL_ATTR_USE_BUFFERED_QUERY：（在MySQL中可用）使用缓冲查询；
		PDO::ATTR_DEFAULT_FETCH_MODE：设置默认的提取模式。
		```