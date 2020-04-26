1- Crie a tabela com 2 famílias de colunas:
a. personal-data
b. professional-data

create 'italians', 'personal-data', 'professional-data'

2. Importe o arquivo via linha de comando

hbase shell /tmp/italians.txt

Agora execute as seguintes operações:
1. Adicione mais 2 italianos mantendo adicionando informações como data de nascimento nas informações pessoais e um atributo de anos de experiência nas informações profissionais;

put 'italians', '11', 'personal-data:name',  'Joao Carlos Haag'

put 'italians', '11', 'personal-data:city',  'Blumenau'

put 'italians', '11', 'personal-data:Nascimento',  '25/05/1992'

put 'italians', '11', 'professional-data:role',  'Analista'

put 'italians', '11', 'professional-data:salary',  '20000'

put 'italians', '11', 'professional-data:Experiencia',  '10 anos'


put 'italians', '12', 'personal-data:name',  'Jaqueline'

put 'italians', '12', 'personal-data:city',  'Blumenau'

put 'italians', '12', 'personal-data:Nascimento',  '28/02/1995'

put 'italians', '12', 'professional-data:role',  'Vendedora'

put 'italians', '12', 'professional-data:salary',  '20000'

put 'italians', '12', 'professional-data:Experiencia',  '7 anos'

2. Adicione o controle de 5 versões na tabela de dados pessoais.

alter 'italians', NAME => 'personal-data', VERSIONS => 5

3. Faça 5 alterações em um dos italianos;

hbase(main):051:0> put 'italians', '12', 'personal-data:city',  'Gaspar'

hbase(main):052:0> put 'italians', '12', 'personal-data:city',  'Pomerode'

hbase(main):053:0> put 'italians', '12', 'personal-data:city',  'Indaial'

hbase(main):054:0> put 'italians', '12', 'personal-data:city',  'Timbo'

hbase(main):055:0> put 'italians', '12', 'personal-data:city',  'Penha'


4. Com o operador get, verifique como o HBase armazenou o histórico.

hbase(main):057:0> get 'italians', '12', {COLUMN => 'personal-data:city', VERSIONS => 5}

COLUMN                                            CELL

 personal-data:city                               timestamp=1587865677055, value=Penha
 
 personal-data:city                               timestamp=1587865666159, value=Timbo
 
 personal-data:city                               timestamp=1587865657823, value=Indaial
 
 personal-data:city                               timestamp=1587865650870, value=Pomerode
 
 personal-data:city                               timestamp=1587865643889, value=Gaspar

5. Utilize o scan para mostrar apenas o nome e profissão dos italianos.

scan 'italians', {COLUMNS => ['personal-data:name', 'professional-data:role']} 

6. Apague os italianos com row id ímpar

hbase(main):014:0> deleteall 'italians', '1'

hbase(main):014:0> deleteall 'italians', '3'

hbase(main):015:0> deleteall 'italians', '5'

hbase(main):016:0> deleteall 'italians', '7'

hbase(main):017:0> deleteall 'italians', '9'

hbase(main):018:0> deleteall 'italians', '11'

7. Crie um contador de idade 55 para o italiano de row id 5
8. Incremente a idade do italiano em 1
