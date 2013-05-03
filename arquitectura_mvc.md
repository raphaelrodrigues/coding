{\rtf1\ansi\ansicpg1252\cocoartf1138\cocoasubrtf510
{\fonttbl\f0\fswiss\fcharset0 Helvetica;\f1\fswiss\fcharset0 ArialMT;\f2\fnil\fcharset0 Verdana;
\f3\fmodern\fcharset0 Courier;\f4\froman\fcharset0 Times-Roman;}
{\colortbl;\red255\green255\blue255;\red38\green38\blue38;\red249\green249\blue249;\red239\green244\blue246;
\red52\green52\blue52;}
{\*\listtable{\list\listtemplateid1\listhybrid{\listlevel\levelnfc23\levelnfcn23\leveljc0\leveljcn0\levelfollow0\levelstartat1\levelspace360\levelindent0{\*\levelmarker \{square\}}{\leveltext\leveltemplateid1\'01\uc0\u9642 ;}{\levelnumbers;}\fi-360\li720\lin720 }{\listname ;}\listid1}}
{\*\listoverridetable{\listoverride\listid1\listoverridecount0\ls1}}
\paperw11900\paperh16840\margl1440\margr1440\vieww10800\viewh8400\viewkind0
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural

\f0\fs24 \cf0 #Guide MVC\
\
\
#MVC\
\
##DAO\
```JAVA\
\
/**\
 * DAO of employee.\
 */\
public interface EmployeeDao extends GenericDao<Employee, Long> \{\
\
    /**\
     * Tries to remove employee from the system.\
     * @param employee Employee to remove\
     * @return \{@code true\} if employee is not assigned to any task\
     * or timesheet. Else \{@code false\}.\
     */\
    boolean removeEmployee(Employee employee);\
\
\}\
```\
\
\
\
\
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural
\cf0 ```JAVA\
\
package org.timesheet.service.impl;\
\
import org.hibernate.Query;\
import org.springframework.stereotype.Repository;\
import org.timesheet.domain.Employee;\
import org.timesheet.service.dao.EmployeeDao;\
\
@Repository('employeeDao')\
public class EmployeeDaoImpl extends HibernateDao<Employee, Long> implements EmployeeDao \{\
\
    @Override\
    public boolean removeEmployee(Employee employee) \{\
        Query employeeTaskQuery = currentSession().createQuery(\
                'from Task t where :id in elements(t.assignedEmployees)');\
        employeeTaskQuery.setParameter('id', employee.getId());\
\
        // employee mustn't be assigned on no task\
        if (!employeeTaskQuery.list().isEmpty()) \{\
            return false;\
        \}\
	\'85\'85..\
\}\
```\
\
#Service\
\pard\pardeftab720\sa400

\f1\fs28 \cf2 As for DAOs, we\'92re pretty much done. One thing left though \'96 our TimesheetService interface. That\'92s the set of business operations that we\'92re intersted in, so let\'92s implement it using Hibernate. We\'92ll put TimesheetServiceImpl class under 
\i org.timesheet.service.impl
\i0  package:\

\f0\fs24 \cf0 \
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural
\cf0 \
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural
\cf0 ```JAVA\
public class TimesheetServiceImpl implements TimesheetService \{\
\
    // dependencies\
    private SessionFactory sessionFactory;\
    private TaskDao taskDao;\
\
    private Random random = new Random();\
\
    @Autowired\
    public void setSessionFactory(SessionFactory sessionFactory) \{\
        this.sessionFactory = sessionFactory;\
    \}\
\
    @Autowired\
    public void setTaskDao(TaskDao taskDao) \{\
        this.taskDao = taskDao;\
    \}\
\
    public SessionFactory getSessionFactory() \{\
        return sessionFactory;\
    \}\
\
    public TaskDao getTaskDao() \{\
        return taskDao;\
    \}\
\
    private Session currentSession() \{\
        return sessionFactory.getCurrentSession();\
    \}\
\
    @Override\
    public Task busiestTask() \{\
        List<Task> tasks = taskDao.list();\
        if (tasks.isEmpty()) \{\
            return null;\
        \}\
        \
        Task busiest = tasks.get(0);\
        for (Task task : tasks) \{\
            if (task.getAssignedEmployees().size() > busiest.getAssignedEmployees().size()) \{\
                busiest = task;\
            \}\
        \}\
        \
        return busiest;\
    \}\
\
\'85\'85..\
\}\
\
```\
\
#DAO / Service\
\pard\pardeftab720

\f2\i\b\fs26 \cf2 \cb3 Person.java
\f0\i0\b0\fs24 \cf0 \cb1 \
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural
\cf0 ```JAVA\
\pard\pardeftab720

\f3 \cf2 \cb4 public class Person implements Serializable \{\
    // This a unique number that will be used during serialization\
    private static final long serialVersionUID = 210120121345L;\
    @Id\
    @GeneratedValue(strategy = GenerationType.AUTO)\
    private int id;\
    @Column private String name;\
    @Column private String address;\
    ...\
\
    @Override\
    public String toString() \{\
      return ToStringBuilder.reflectionToString(this);\}\
\
    //setters & getters
\f0 \cf0 \cb1 \
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural
\cf0 \
```\
\
\pard\pardeftab720

\f2\i\b\fs26 \cf2 \cb3 PersonDAO and Jdbc_PersonDAO
\f0\i0\b0\fs24 \cf0 \cb1 \
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural
\cf0 \
```JAVA\
\
\pard\pardeftab720

\f3 \cf2 \cb4 package net.jdj.coffee.dao;\
...\
public interface PersonDAO \{\
  public List<Person> listAll(int startPage, int pageSize);\
  public int PersonCount();\
\}
\f0 \cf0 \cb1 \
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural
\cf0 ```\
\
```JAVA\
\
\pard\pardeftab720

\f3 \cf2 \cb4 package net.jdj.coffee.dao;\
...\
public class Jdbc_PersonDAO implements PersonDAO \{\
\
  /** Logger for this class and subclasses */\
  protected final Log logger = LogFactory.getLog(getClass());\
\
  private SimpleJdbcTemplate simpleJdbcTemplate;\
  public void setDataSource(DataSource dataSource) \{\
    this.simpleJdbcTemplate = new SimpleJdbcTemplate(dataSource);\}\
\
  public List<Person> listAll(int startPage, int pageSize) \{\
    int temp = startPage * pageSize;\
    String upit = "select id, name, address, gender, dob, email, mobile, phone from PERSON " +\
      "where id >= " + startPage + " and id < " + temp;\
    List<Person> persons = new ArrayList<Person>();\
        \
    List<Map<String, Object>> rows = simpleJdbcTemplate.queryForList(upit);\
    for (Map<String, Object> row : rows)\{\
      Person person = new Person();\
      person.setId((Integer)(row.get("ID")));\
      person.setName((String)(row.get("NAME")));\
      person.setAddress((String)(row.get("ADDRESS")));\
      person.setGender((String)(row.get("GENDER")));\
      person.setDob((Date)(row.get("DOB")));\
      person.setEmail((String)(row.get("EMAIL")));\
      person.setMobile((String)(row.get("MOBILE")));\
      person.setPhone((String)(row.get("PHONE")));\
      persons.add(person); \}\
    return persons;\}\
  public int PersonCount() \{\
    return 5;\}\
\}
\f0 \cf0 \cb1 \
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural
\cf0 ```\
\
\pard\pardeftab720

\f2\i\b\fs26 \cf2 \cb3 PersonService
\f0\i0\b0\fs24 \cf0 \cb1 \
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural
\cf0 ```JAVA\
\pard\pardeftab720

\f3 \cf2 \cb4 package net.jdj.coffee.service;\
\'85\
public interface PersonService extends Serializable\{ ...\}
\f0 \cf0 \cb1 \
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural
\cf0 ```\
\
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural
\cf0 \
\
\
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural
\cf0 ```JAVA\
\
\pard\pardeftab720

\f3 \cf2 \cb4 public class Simple_PersonService implements PersonService \{\
  /** Logger for this class and subclasses */\
  protected final Log logger = LogFactory.getLog(getClass());\
  // This a unique number that will be used during serialization\
  private static final long serialVersionUID = 130220121406L;\
  private PersonDAO personDAO;\
  public void setpersonDAO(PersonDAO personDAO) \{ \
    this.personDAO = personDAO; \}\
  @Transactional\
  public List<Person> getPersonAll(int startPage, int pageSize) \{ \
    return personDAO.listAll(startPage, pageSize); \} \
  @Transactional\
  public int getPersonCount() \{\
    return personDAO.PersonCount(); \} \
\}
\f0 \cf0 \cb1 \
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural
\cf0 ```\
\pard\pardeftab720

\f2\i\b\fs26 \cf2 \cb3 PersonController
\f0\i0\b0\fs24 \cf0 \cb1 \
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural
\cf0 \
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural
\cf0 ```JAVA\
\pard\pardeftab720

\f3 \cf2 \cb4 public class PersonController \{\
\}\
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural

\f0 \cf0 \cb1 ```\
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural
\cf0 \
\pard\pardeftab720\sa400

\f1\fs28 \cf2 That\'92s it, we have whole CRUD functionality for our employees. Let\'92s recap the basic steps what we just did:\
\pard\tx220\tx720\pardeftab720\li720\fi-720
\ls1\ilvl0\cf2 {\listtext	\uc0\u9642 	}Added EmployeeController class\
{\listtext	\uc0\u9642 	}Create resources folder in web root for static content\
{\listtext	\uc0\u9642 	}Added mapping for default servlet in web.xml\
{\listtext	\uc0\u9642 	}Added styles.css to resources folder\
{\listtext	\uc0\u9642 	}Configured POST-DELETE tunneling with filter in web.xml\
{\listtext	\uc0\u9642 	}Downloaded jQuery.js and added to our resources folder\
{\listtext	\uc0\u9642 	}Added employess/list.jsp page\
{\listtext	\uc0\u9642 	}Added employess/view.jsp page\
{\listtext	\uc0\u9642 	}Added employess/new.jsp page\
{\listtext	\uc0\u9642 	}Added employees/delete-error.jsp page
\f0\fs24 \cf0 \
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural
\cf0 \
\
\
#
\f1\b\fs36 \cf5 Defini\'e7\'e3o por camadas\
##BO (Business Object)\
\pard\pardeftab720

\b0\fs26 \cf5 Objeto de neg\'f3cios (BO) s\'e3o usados em programa\'e7\'e3o orientada a objeto, ele \'e9 uma representa\'e7\'e3o de partes de um neg\'f3cio, este pode representar, por exemplo, uma pessoa, lugar, evento, processo de neg\'f3cio ou conceito.\
\
Embora as classes podem conter execu\'e7\'e3o ou comportamentos de gest\'e3o, um objeto de neg\'f3cio \'e9 geralmente inerte a conjuntos de titula\'e7\'e3o de vari\'e1veis de inst\'e2ncia ou propriedades. Um BO tamb\'e9m pode fazer solicita\'e7\'f5es de dados do cliente para o Data Access Object (DAO)
\b\fs36 \cf5 \
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural
\cf5 \
##
\fs20 \cf5 Classe BO
\fs36 \cf5 \
\pard\pardeftab720

\fs20 \cf5 `
\fs36 \cf5 ``JAVA\
\pard\pardeftab720

\f4\b0\fs20 \cf5 public class PessoaBO \{\'a0
\f1\fs26 \cf5 \

\f4\fs20 \cf5 \'a0\'a0\'a0 public void novaPessoa(Pessoa pessoa) \{
\fs26  
\fs20 \
\'a0\'a0\'a0 \'a0\'a0\'a0 new PessoaDAO().savePessoa(pessoa)\
\'a0\'a0\'a0 \}\
\'a0\'a0\'a0 public List<Pessoa> PegarPessoas()\{
\fs26  
\fs20 \
\'a0\'a0\'a0 \'a0\'a0\'a0 new PessoaDAO().getPessoas();\
\'a0\'a0\'a0 \} \
\} 
\f1\b\fs36 \cf5 \
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural
\cf5 ```\
\
##BEAN\
\pard\pardeftab720

\b0\fs26 \cf5 Praticamente s\'e3o classes escritas de acordo com uma conven\'e7\'e3o em particular. S\'e3o usados para encapsular muitos objetos em um \'fanico objeto (o bean), assim eles podem ser transmitidos como um \'fanico objeto em vez de v\'e1rios objetos individuais. O JavaBean \'e9 um Objeto Java que \'e9 serializavel, possui um construtor nulo e permite acesso \'e0s suas propriedades atrav\'e9s de m\'e9todos getter e setter.\

\b\fs36 \cf5 \
##
\fs20 \cf5 Classe BEAN\
```JAVA\
\pard\pardeftab720

\f4\b0 \cf5 public class Pessoa \{
\fs26 \

\fs20 \'a0\'a0\'a0 private String nome;\'a0
\fs26 \

\fs20 \'a0\'a0\'a0 private String idade;\'a0
\fs26 \
\

\fs20 \'a0\'a0\'a0 public String getNome() \{\'a0
\fs26 \

\fs20 \'a0\'a0\'a0 \'a0\'a0\'a0 return nome;\'a0
\fs26 \

\fs20 \'a0\'a0\'a0 \}\'a0
\fs26 \

\fs20 \'a0\'a0\'a0 public void setNome(String nome) \{\'a0
\fs26 \

\fs20 \'a0\'a0\'a0 \'a0\'a0\'a0 this.nome = nome;\'a0
\fs26 \

\fs20 \'a0\'a0\'a0 \}\'a0
\fs26 \

\fs20 \'a0\'a0\'a0 public String getIdade() \{\'a0
\fs26 \

\fs20 \'a0\'a0\'a0\'a0 \'a0\'a0\'a0 return idade;\'a0
\fs26 \

\fs20 \'a0\'a0\'a0 \}\'a0
\fs26 \

\fs20 \'a0\'a0\'a0 public void setIdade(String idade) \{\'a0
\fs26 \

\fs20 \'a0\'a0\'a0 \'a0\'a0\'a0 this.idade = idade;\'a0
\fs26 \

\fs20 \'a0\'a0\'a0 \}\'a0
\fs26 \

\fs20 \}
\f1\b \cf5 \
\
```
\fs36 \cf5 \
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural
\cf5 \
##DAO \
\pard\pardeftab720

\b0\fs26 \cf5 O DAO funciona como um tradutor dos mundos. Suponha um banco relacional. O DAO deve saber buscar os dados do banco e converter em objetos para ser usado pela aplica\'e7\'e3o. Semelhantemente, deve saber como pegar os objetos, converter em instru\'e7\'f5es SQL e mandar para o banco de dados. \'c9 assim que um DAO trabalha.\
\
Geralmente, temos um DAO para cada objeto do dom\'ednio do sistema (Produto, Cliente, Compra, etc.), ou ent\'e3o para cada m\'f3dulo, ou conjunto de entidades fortemente relacionadas.\
\
\pard\pardeftab720

\b\fs20 \cf5 Classe DAO\
\pard\pardeftab720

\fs36 \cf5 \
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural

\f0\b0\fs24 \cf0 \
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural
\cf0 ```JAVA\
\pard\pardeftab720

\f4\fs20 \cf5 public class PessoaDAO \{
\f1\fs26 \cf5 \

\f4\fs20 \cf5 \'a0\'a0\'a0 public void savePessoa(Pessoa pessoa)\{
\fs26  
\fs20 \
\'a0\'a0\'a0 \'a0\'a0\'a0 //CODIFICA\'c7\'c3O AKI \
\'a0\'a0\'a0 \}\'a0
\f1\fs26 \cf5 \

\f4\fs20 \cf5 \'a0\'a0\'a0 public void deletePessoa(Pessoa pessoa)\{ \
\'a0\'a0\'a0 \'a0\'a0\'a0 //CODIFICA\'c7\'c3O AKI \
\'a0\'a0\'a0 \}\'a0
\f1\fs26 \cf5 \

\f4\fs20 \cf5 \'a0\'a0\'a0 public List<Pessoa> getPessoas()\{
\fs26  
\fs20 \
\'a0\'a0\'a0 \'a0\'a0\'a0 //CODIFICA\'c7\'c3O AKI \
\'a0\'a0\'a0 \'a0\'a0\'a0 return list;\'a0
\f1\fs26 \cf5 \

\f4\fs20 \cf5 \'a0\'a0\'a0 \}\
\}
\f0\fs24 \cf0 \
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural
\cf0 ```}