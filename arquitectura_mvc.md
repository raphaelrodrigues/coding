#Guide MVC


#MVC

##DAO
```JAVA

/**
 * DAO of employee.
 */
public interface EmployeeDao extends GenericDao<Employee, Long> {

    /**
     * Tries to remove employee from the system.
     * @param employee Employee to remove
     * @return {@code true} if employee is not assigned to any task
     * or timesheet. Else {@code false}.
     */
    boolean removeEmployee(Employee employee);

}
```




```JAVA

public class EmployeeDaoImpl extends HibernateDao<Employee, Long> implements EmployeeDao {

    @Override
    public boolean removeEmployee(Employee employee) {
        Query employeeTaskQuery = currentSession().createQuery(
                'from Task t where :id in elements(t.assignedEmployees)');
        employeeTaskQuery.setParameter('id', employee.getId());

        // employee mustn't be assigned on no task
        if (!employeeTaskQuery.list().isEmpty()) {
            return false;
        }
	……..
}
```

#Service
As for DAOs, we’re pretty much done. One thing left though – our TimesheetService interface. That’s the set of business operations that we’re intersted in, so let’s implement it using Hibernate. We’ll put TimesheetServiceImpl class under org.timesheet.service.impl package:


```JAVA
public class TimesheetServiceImpl implements TimesheetService {

    // dependencies
    private SessionFactory sessionFactory;
    private TaskDao taskDao;

    private Random random = new Random();

    @Autowired
    public void setSessionFactory(SessionFactory sessionFactory) {
        this.sessionFactory = sessionFactory;
    }

    @Autowired
    public void setTaskDao(TaskDao taskDao) {
        this.taskDao = taskDao;
    }

    public SessionFactory getSessionFactory() {
        return sessionFactory;
    }

    public TaskDao getTaskDao() {
        return taskDao;
    }

    private Session currentSession() {
        return sessionFactory.getCurrentSession();
    }

    @Override
    public Task busiestTask() {
        List<Task> tasks = taskDao.list();
        if (tasks.isEmpty()) {
            return null;
        }
        
        Task busiest = tasks.get(0);
        for (Task task : tasks) {
            if (task.getAssignedEmployees().size() > busiest.getAssignedEmployees().size()) {
                busiest = task;
            }
        }
        
        return busiest;
    }

……..
}

```

#DAO / Service
Person.java
```JAVA
public class Person implements Serializable {
    // This a unique number that will be used during serialization
    private static final long serialVersionUID = 210120121345L;
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private int id;
    @Column private String name;
    @Column private String address;
    ...

    @Override
    public String toString() {
      return ToStringBuilder.reflectionToString(this);}

    //setters & getters

```

PersonDAO and Jdbc_PersonDAO

```JAVA

package net.jdj.coffee.dao;
...
public interface PersonDAO {
  public List<Person> listAll(int startPage, int pageSize);
  public int PersonCount();
}
```

```JAVA

package net.jdj.coffee.dao;
...
public class Jdbc_PersonDAO implements PersonDAO {

  /** Logger for this class and subclasses */
  protected final Log logger = LogFactory.getLog(getClass());

  private SimpleJdbcTemplate simpleJdbcTemplate;
  public void setDataSource(DataSource dataSource) {
    this.simpleJdbcTemplate = new SimpleJdbcTemplate(dataSource);}

  public List<Person> listAll(int startPage, int pageSize) {
    int temp = startPage * pageSize;
    String upit = "select id, name, address, gender, dob, email, mobile, phone from PERSON " +
      "where id >= " + startPage + " and id < " + temp;
    List<Person> persons = new ArrayList<Person>();
        
    List<Map<String, Object>> rows = simpleJdbcTemplate.queryForList(upit);
    for (Map<String, Object> row : rows){
      Person person = new Person();
      person.setId((Integer)(row.get("ID")));
      person.setName((String)(row.get("NAME")));
      person.setAddress((String)(row.get("ADDRESS")));
      person.setGender((String)(row.get("GENDER")));
      person.setDob((Date)(row.get("DOB")));
      person.setEmail((String)(row.get("EMAIL")));
      person.setMobile((String)(row.get("MOBILE")));
      person.setPhone((String)(row.get("PHONE")));
      persons.add(person); }
    return persons;}
  public int PersonCount() {
    return 5;}
}
```

PersonService
```JAVA
package net.jdj.coffee.service;
…
public interface PersonService extends Serializable{ ...}
```




```JAVA

public class Simple_PersonService implements PersonService {
  /** Logger for this class and subclasses */
  protected final Log logger = LogFactory.getLog(getClass());
  // This a unique number that will be used during serialization
  private static final long serialVersionUID = 130220121406L;
  private PersonDAO personDAO;
  public void setpersonDAO(PersonDAO personDAO) { 
    this.personDAO = personDAO; }
  @Transactional
  public List<Person> getPersonAll(int startPage, int pageSize) { 
    return personDAO.listAll(startPage, pageSize); } 
  @Transactional
  public int getPersonCount() {
    return personDAO.PersonCount(); } 
}
```
PersonController

```JAVA
public class PersonController {
}
```

That’s it, we have whole CRUD functionality for our employees. Let’s recap the basic steps what we just did:
	▪	Added EmployeeController class
	▪	Create resources folder in web root for static content
	▪	Added mapping for default servlet in web.xml
	▪	Added styles.css to resources folder
	▪	Configured POST-DELETE tunneling with filter in web.xml
	▪	Downloaded jQuery.js and added to our resources folder
	▪	Added employess/list.jsp page
	▪	Added employess/view.jsp page
	▪	Added employess/new.jsp page
	▪	Added employees/delete-error.jsp page



#Definição por camadas
##BO (Business Object)
Objeto de negócios (BO) são usados em programação orientada a objeto, ele é uma representação de partes de um negócio, este pode representar, por exemplo, uma pessoa, lugar, evento, processo de negócio ou conceito.

Embora as classes podem conter execução ou comportamentos de gestão, um objeto de negócio é geralmente inerte a conjuntos de titulação de variáveis de instância ou propriedades. Um BO também pode fazer solicitações de dados do cliente para o Data Access Object (DAO)

##Classe BO
```JAVA
public class PessoaBO { 
    public void novaPessoa(Pessoa pessoa) { 
	 new PessoaDAO().savePessoa(pessoa)
     }
    public List<Pessoa> PegarPessoas(){ 
          new PessoaDAO().getPessoas();
     } 
} 
```

##BEAN
Praticamente são classes escritas de acordo com uma convenção em particular. São usados para encapsular muitos objetos em um único objeto (o bean), assim eles podem ser transmitidos como um único objeto em vez de vários objetos individuais. O JavaBean é um Objeto Java que é serializavel, possui um construtor nulo e permite acesso às suas propriedades através de métodos getter e setter.

##Classe BEAN
```JAVA
public class Pessoa {
    private String nome;
    private String idade;
   
    public String getNome() {
 	return nome;
     }
     public void setNome(String nome) {
	 this.nome = nome;
     }
     public String getIdade() {
	 return idade;
    }
    public void setIdade(String idade) {
	 this.idade = idade;
     }
}

```

##DAO 
O DAO funciona como um tradutor dos mundos. Suponha um banco relacional. O DAO deve saber buscar os dados do banco e converter em objetos para ser usado pela aplicação. Semelhantemente, deve saber como pegar os objetos, converter em instruções SQL e mandar para o banco de dados. É assim que um DAO trabalha.

Geralmente, temos um DAO para cada objeto do domínio do sistema (Produto, Cliente, Compra, etc.), ou então para cada módulo, ou conjunto de entidades fortemente relacionadas.

Classe DAO


```JAVA
public class PessoaDAO {
     public void savePessoa(Pessoa pessoa){ 
	 //CODIFICAÇÃO AKI 
    }
    public void deletePessoa(Pessoa pessoa){ 
 	 //CODIFICAÇÃO AKI 
    }
    public List<Pessoa> getPessoas(){ 
	 //CODIFICAÇÃO AKI 
	 return list; 
     }
}
```
