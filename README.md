# Техническое задание №1
Выполнено задание по написанию скрипта
## Легенда задания
Вам нужно написать скрипт на bash, который на вход принимает два параметра - две директории (входная директория и выходная директория). Во входной директории могут находиться как файлы, так и вложенные директории (внутри которых тоже могут быть как файлы, так и папки) - уровень вложенности может быть любой. Задача скрипта - "обойти" входную директорию и скопировать все файлы из нее (и из всех сложенных директорий) в выходную директорию - но уже без иерархии, а просто все файлы - внутри выходной директории.
## Код скрипта
`
#!/bin/bash
input_dir=$1
output_dir=$2

find "$input_dir" -type f -exec bash -c '
    old_name=$2
    path="${old_name#$0/}"   
    new_name="${path//\//_}"
    cp "$old_name" "$1/$new_name"
' $input_dir $output_dir {} \;   
`
## Описание алгоритма
1. При запуске скрипта на вход передаются два параметра: две директории (входная директория и выходная директория). Они записываются в переменные input_dir и output_dir соответственно.
2. С помощью команды find -type f мы находим все файлы, находящиеся во входной директории и всех ее вложенных директориях.
3. Для каждого из найденных файлов с помощью параметра -exec мы выполняем скрипт, в который передается три параметра: входная директория, выходная директория и найденный файл (его путь передается с помощью фигурных скобок {}).
4. В переменную old_name передается третий параметр, а именно найденный файл. Создается новая переменная path, которая будет содержать путь внутри входной директории до текущего файла. Далее создается переменная new_name, в которую записывается значение переменной path, в котором все символы "/" меняются на "_". Это делается для того, чтобы все файлы размещались в выходной директории без иерархии и при переносе файлов в выходную директорию не возникала проблема с файлами, имеющими одинаковые названия. В конце скрипта производится копирование файла в выходную директорию с новым именем. 
