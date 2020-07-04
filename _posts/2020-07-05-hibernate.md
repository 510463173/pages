---
title: Hibernate
tags: ["Java", "ORM"]
article_header:
  type: cover
  image:
    src: /images/002.jpg
---
Hibernate
---

Table of Contents
<!-- TOC -->

- [1. 领域模型](#1-领域模型)
    - [1.1. 映射类型](#11-映射类型)
        - [1.1.1. 值类型](#111-值类型)
        - [1.1.2. 实体类型](#112-实体类型)
    - [1.2. 命名策略](#12-命名策略)
    - [1.3. 基本类型](#13-基本类型)
        - [1.3.1. 自定义基本类型](#131-自定义基本类型)
        - [1.3.2. 枚举类型映射](#132-枚举类型映射)
        - [1.3.3. 映射 LOB](#133-映射-lob)
            - [1.3.3.1. 映射CLOB](#1331-映射clob)
            - [1.3.3.2. 映射BLOB](#1332-映射blob)
        - [1.3.4. 时间类型的映射](#134-时间类型的映射)
        - [1.3.5. @Generated](#135-generated)
        - [1.3.6. @GeneratorType](#136-generatortype)
        - [1.3.7. @ValueGenerationType](#137-valuegenerationtype)
        - [1.3.8. 列转换器](#138-列转换器)
        - [1.3.9. 保留字](#139-保留字)
    - [1.4. 可嵌入类型](#14-可嵌入类型)
    - [1.5. 实体类型](#15-实体类型)
        - [1.5.1. final 类](#151-final-类)
        - [1.5.2. 无参构造函数](#152-无参构造函数)
        - [1.5.3. 声明持久性属性的获取器和设置器](#153-声明持久性属性的获取器和设置器)
        - [1.5.4. 映射实体](#154-映射实体)
        - [1.5.5. 实现持久化对象的 equals() hashCode()](#155-实现持久化对象的-equals-hashcode)
        - [1.5.6. 实体映射到 SQL 查询](#156-实体映射到-sql-查询)
        - [1.5.7. 自定义代理类](#157-自定义代理类)
        - [1.5.8. 动态代理](#158-动态代理)
        - [1.5.9. 自定义持久化类](#159-自定义持久化类)
        - [1.5.10. 访问策略](#1510-访问策略)
    - [1.6. 标识符](#16-标识符)
        - [1.6.1. 简单标识符](#161-简单标识符)
        - [1.6.2. 复合标识符](#162-复合标识符)
        - [1.6.3. @EmbeddedId](#163-embeddedid)
        - [1.6.4. @IdClass](#164-idclass)
        - [1.6.5. 具有关联的复合标识符](#165-具有关联的复合标识符)
        - [1.6.6. 标识符和自动生成属性](#166-标识符和自动生成属性)
        - [1.6.7. 生成标识符的值](#167-生成标识符的值)
        - [1.6.8. UUID 生成器](#168-uuid-生成器)
        - [1.6.9. 标识符优化器](#169-标识符优化器)
        - [1.6.10. @GenericGenerator](#1610-genericgenerator)
        - [1.6.11. 派生标识符](#1611-派生标识符)
        - [1.6.12. @RowId](#1612-rowid)
    - [1.7. 关联关系](#17-关联关系)
        - [1.7.1. @ManyToOne](#171-manytoone)
        - [1.7.2. @OneToMany](#172-onetomany)
            - [1.7.2.1. 单向](#1721-单向)
            - [1.7.2.2. 双向](#1722-双向)
        - [1.7.3. @OneToOne](#173-onetoone)
            - [1.7.3.1. 单向](#1731-单向)
            - [1.7.3.2. 双向](#1732-双向)
        - [1.7.4. @ManyToMany](#174-manytomany)
            - [1.7.4.1. 单向](#1741-单向)
            - [1.7.4.2. 无链接实体的双向](#1742-无链接实体的双向)
            - [1.7.4.3. 有链接实体的双向](#1743-有链接实体的双向)
    - [1.8. @NaturalId](#18-naturalid)
        - [1.8.1. 映射](#181-映射)
        - [1.8.2. 使用 @NaturalId 加载](#182-使用-naturalid-加载)
        - [1.8.3. 可变性和缓存](#183-可变性和缓存)
    - [1.9. 继承](#19-继承)
        - [1.9.1. Mapped Super class](#191-mapped-super-class)
        - [1.9.2. Single table](#192-single-table)
        - [1.9.3. Joined Table](#193-joined-table)
        - [1.9.4. Table Per Class](#194-table-per-class)
- [2. 引导程序](#2-引导程序)
    - [2.1. 构建 ServiceRegistry](#21-构建-serviceregistry)
    - [2.2. 监听](#22-监听)
    - [2.3. 配置元数据](#23-配置元数据)
    - [2.4. SessionFactroy](#24-sessionfactroy)
- [3. Schema Generation](#3-schema-generation)
    - [3.1. 导入脚本文件](#31-导入脚本文件)
    - [3.2. 数据库层的检查](#32-数据库层的检查)
    - [3.3. 数据库列的默认值](#33-数据库列的默认值)
    - [3.4. 唯一约束](#34-唯一约束)
    - [3.5. 索引](#35-索引)
- [4. 持久性上下文](#4-持久性上下文)
    - [4.1. 使实体持久化](#41-使实体持久化)
    - [4.2. 删除实体](#42-删除实体)
    - [4.3. 获取实体](#43-获取实体)

<!-- /TOC -->

总览
---
Hibernate是一种用于Java环境的对象/关系映射(ORM)解决方案。有效地“位于” Java应用程序数据访问层和关系数据库之间。

# 1. 领域模型 #
Hibernate实现Java Persistence API规范。使用Hibernate的应用程序会为此目的使用其专有的XML映射文件格式。随着JPA的到来，现在大多数信息都以注解（和/或标准化XML格式）可在ORM / JPA提供程序之间移植的方式进行定义。对于JPA不支持的Hibernate映射功能，我们将更喜欢Hibernate扩展注解。

## 1.1. 映射类型 ##
Hibernate将类型分为两类：值类型、实体类型
```
SQL:
create table Contact (
    id integer not null,
    first varchar(255),
    last varchar(255),
    middle varchar(255),
    notes varchar(255),
    starred boolean not null,
    website varchar(255),
    primary key (id)
)

Java:
@Entity(name = "Contact")
public static class Contact {

	@Id
	private Integer id;

	private Name name;

	private String notes;

	private URL website;

	private boolean starred;

	//Getters and setters are omitted for brevity
}

@Embeddable
public class Name {

	private String first;

	private String middle;

	private String last;

	// getters and setters omitted
}
```
### 1.1.1. 值类型 ###
值类型进一步分为三个子类别：基本类型、可嵌入类型（例如上述例子中的Name）、集合类型

### 1.1.2. 实体类型 ###
实体根据其唯一标识符的性质独立于其他对象而存在，而值则不存在。实体是域模型类，使用唯一标识符与数据库表中的行相关。由于需要唯一标识符，因此实体独立存在并定义自己的生命周期。例如上述例子中的Contact。

## 1.2. 命名策略 ##
从Hibernate5开始，从领域模型到关系型数据库的名称映射分成两个步骤：
1. 从域模型映射中确定适当的逻辑名。逻辑名可以由用户明确指定（例如使用@Column或 @Table），也可以由Hibernate通过ImplicitNamingStrategy协定隐式确定。
2. 将此逻辑名称解析为由PhysicalNamingStrategy合同定义的物理名称。

隐式命名配置：
```
hibernate.implicit_naming_strategy:
- default //jpa的别名
- jpa // jpa2.0标准的命名策略
- legacy-hbm // 兼容旧版本的命名策略
- legacy-jpa // jpa1.0规范
- coponent-path //类似default，区别在于采用全路径
- 可以指定一个继承ImplicitNamingStrategy的类
```
物理命名策略是用来把逻辑名称转换成在数据库中的具体表名或列名，默认情况下直接返回逻辑名称。如果需要在表名或者列名前加前缀，例如逻辑名称为account的逻辑名称需要映射到tab_account的表名时，可以继承PhysicalNamingStrategy来自定义实现，并在该属性 `hibernate.implicit_naming_strategy` 指定自定义的类。

## 1.3. 基本类型 ##
基本值类型通常将单个数据库列映射到单个非聚合Java类型，
[点此查看官网列出的基本类型](https://docs.jboss.org/hibernate/orm/5.4/userguide/html_single/Hibernate_User_Guide.html#basic-provided)。org.hibernate.type.BasicTypeRegistry是用来管理各种基本类型类，管理一个以注册名称为key，BasicType为value的Map对象。通过这个对象hibernate才能准确的将java类型映射成sql类型。

>JPA规范严格将可以标记为基本的Java类型限制在以下列表中，如果需要提供程序的可移植性，则应仅遵循这些基本类型：
>- Java基本类型（boolean，int，等）
>- 包装的原始类型（java.lang.Boolean，java.lang.Integer等）
>- java.lang.String
>- java.math.BigInteger
>- java.math.BigDecimal
>- java.util.Date
>- java.util.Calendar
>- java.sql.Date
>- java.sql.Time
>- java.sql.Timestamp
>- byte[] or Byte[]
>- char[] or Character[]
>- enums
>- 实现的任何其他类型Serializable（JPA对Serializable类型的“支持” 是将其状态直接序列化到数据库）。

@Column(name="...") 指定列名
@Entity(name="...") 指定表名

**显示指定基本类型**
如果需要对特定属性进行不同的类型处理，需要加上注解@org.hibernate.annotations.Type(type = ...), type可以是：
- 任何实现org.hibernate.type.Type的全限定名称
- 任何在BasicTypeRegistry已注册的key值
- 任何已知类型定义的名称

```
@Entity(name = "Product")
public class Product {
	@Id
	private Integer id;

	private String sku;

	@org.hibernate.annotations.Type( type = "nstring" )
	private String name;

	@org.hibernate.annotations.Type( type = "materialized_nclob" )
	private String description;
}
```

### 1.3.1. 自定义基本类型 ###
有两种方式可以自定义一个基本类型：
- 实现BasicType接口（或者AbstractStandardBasicType，AbstractSingleColumnStandardBasicType）并注册类型
- 实现UserType接口（无需注册）

一、实现AbstractSingleColumnStandardBasicType接口，
定义一个BitSetType，用来完成从Java类型BitSet到SQL类型VARCHAR的映射：
```
public class BitSetType extends AbstractSingleColumnStandardBasicType<BitSet> implements DiscriminatorType<BitSet> {

    public static final BitSetType INSTANCE = new BitSetType();

    public BitSetType() {
        super( VarcharTypeDescriptor.INSTANCE, BitSetTypeDescriptor.INSTANCE );
    }

    @Override
    public BitSet stringToObject(String xml) throws Exception {
        return fromString( xml );
    }

    @Override
    public String objectToSQLString(BitSet value, Dialect dialect) throws Exception {
        return toString( value );
    }

    @Override
    public String getName() {
        return "bitset";
    }

}
```
这里面的构造方法里需要两个类，属于 sqlTypeDescriptor 的 VarcharTypeDescriptor 和 javaTypeDescriptor BitSetTypeDescriptor，需要自己实现用于BitSet的 javaTypeDescriptor。
```
public class BitSetTypeDescriptor extends AbstractTypeDescriptor<BitSet> {

    private static final String DELIMITER = ",";

    public static final BitSetTypeDescriptor INSTANCE = new BitSetTypeDescriptor();

    public BitSetTypeDescriptor() {
        super( BitSet.class );
    }

    @Override
    public String toString(BitSet value) {
        StringBuilder builder = new StringBuilder();
        for ( long token : value.toLongArray() ) {
            if ( builder.length() > 0 ) {
                builder.append( DELIMITER );
            }
            builder.append( Long.toString( token, 2 ) );
        }
        return builder.toString();
    }

    @Override
    public BitSet fromString(String string) {
        if ( string == null || string.isEmpty() ) {
            return null;
        }
        String[] tokens = string.split( DELIMITER );
        long[] values = new long[tokens.length];

        for ( int i = 0; i < tokens.length; i++ ) {
            values[i] = Long.valueOf( tokens[i], 2 );
        }
        return BitSet.valueOf( values );
    }

    @SuppressWarnings({"unchecked"})
    public <X> X unwrap(BitSet value, Class<X> type, WrapperOptions options) {
        if ( value == null ) {
            return null;
        }
        if ( BitSet.class.isAssignableFrom( type ) ) {
            return (X) value;
        }
        if ( String.class.isAssignableFrom( type ) ) {
            return (X) toString( value);
        }
        throw unknownUnwrap( type );
    }

    public <X> BitSet wrap(X value, WrapperOptions options) {
        if ( value == null ) {
            return null;
        }
        if ( String.class.isInstance( value ) ) {
            return fromString( (String) value );
        }
        if ( BitSet.class.isInstance( value ) ) {
            return (BitSet) value;
        }
        throw unknownWrap( value.getClass() );
    }
}
```
unwrap传递时方法用于将BitSet作为PreparedStatement绑定参数，而wrap方法用于将JDBC列值转变为实际的映射对象类型。可以在应用初始化时执行以下代码注册类型。
```
configuration.registerTypeContributor( (typeContributions, serviceRegistry) -> {
	typeContributions.contributeType( BitSetType.INSTANCE );
} );
```
实际使用时：
```
@Type(type = "bitset")
private BitSet bitSet;
```
使用注解@TypeDef就可以省略注册的过程，直接使用：
```
@Entity(name = "Product")
@TypeDef(
	name = "bitset",
	defaultForType = BitSet.class,
	typeClass = BitSetType.class
)
public static class Product {

	@Id
	private Integer id;

	private BitSet bitSet;

	//Getters and setters are omitted for brevity
}
```
二、实现UserType接口
```
public class BitSetUserType implements UserType {

	public static final BitSetUserType INSTANCE = new BitSetUserType();

    private static final Logger log = Logger.getLogger( BitSetUserType.class );

    @Override
    public int[] sqlTypes() {
        return new int[] {StringType.INSTANCE.sqlType()};
    }

    @Override
    public Class returnedClass() {
        return BitSet.class;
    }

    @Override
    public boolean equals(Object x, Object y)
			throws HibernateException {
        return Objects.equals( x, y );
    }

    @Override
    public int hashCode(Object x)
			throws HibernateException {
        return Objects.hashCode( x );
    }

    @Override
    public Object nullSafeGet(
            ResultSet rs, String[] names, SharedSessionContractImplementor session, Object owner)
            throws HibernateException, SQLException {
        String columnName = names[0];
        String columnValue = (String) rs.getObject( columnName );
        log.debugv("Result set column {0} value is {1}", columnName, columnValue);
        return columnValue == null ? null :
				BitSetTypeDescriptor.INSTANCE.fromString( columnValue );
    }

    @Override
    public void nullSafeSet(
            PreparedStatement st, Object value, int index, SharedSessionContractImplementor session)
            throws HibernateException, SQLException {
        if ( value == null ) {
            log.debugv("Binding null to parameter {0} ",index);
            st.setNull( index, Types.VARCHAR );
        }
        else {
            String stringValue = BitSetTypeDescriptor.INSTANCE.toString( (BitSet) value );
            log.debugv("Binding {0} to parameter {1} ", stringValue, index);
            st.setString( index, stringValue );
        }
    }

    @Override
    public Object deepCopy(Object value)
			throws HibernateException {
        return value == null ? null :
            BitSet.valueOf( BitSet.class.cast( value ).toLongArray() );
    }

    @Override
    public boolean isMutable() {
        return true;
    }

    @Override
    public Serializable disassemble(Object value)
			throws HibernateException {
        return (BitSet) deepCopy( value );
    }

    @Override
    public Object assemble(Serializable cached, Object owner)
			throws HibernateException {
        return deepCopy( cached );
    }

    @Override
    public Object replace(Object original, Object target, Object owner)
			throws HibernateException {
        return deepCopy( original );
    }
}
```

### 1.3.2. 枚举类型映射 ###
最初的JPA兼容映射枚举方法是通过 @Enumerated 或 @MapKeyEnumerated 映射键注解进行的。EnumType.ORDINAL 表示会把枚举类中枚举值的序数位置存进数据库（从0开始），EnumType.STRING 表示会把枚举类型名称存进数据库，如果是空值，都是存NULL。
```
@Enumerated(EnumType.ORDINAL 或者 EnumType.STRING)
@Column(name = "phone_type")
private PhoneType type;
```
如果要满足以下这种情况，把 Gender 枚举类型的 code 值存进数据库，需要使用 AttributeConverter。
```
public enum Gender {
    MALE( 'M' ),
    FEMALE( 'F' );

    private final char code;
    ...
}
```
```
@Entity(name = "Person")
public class Person {

	@Convert( converter = GenderConverter.class )
	public Gender gender;

    ...
}
```
```
@Converter
public class GenderConverter
		implements AttributeConverter<Gender, Character> {

	public Character convertToDatabaseColumn( Gender value ) {
		if ( value == null ) {
			return null;
		}

		return value.getCode();
	}

	public Gender convertToEntityAttribute( Character value ) {
		if ( value == null ) {
			return null;
		}

		return Gender.fromCode( value );
	}
}
```
也可以通过自定义类型的定义一个GenderType的类型，在unwrap和wrap方法里处理数据库类型和java类型的转换。

### 1.3.3. 映射 LOB ###

#### 1.3.3.1. 映射CLOB ####
使用 @Lob 注解和 java.sql.Clob 类型来映射（也可以用String 或 char[]）。
```
@Entity(name = "Product")
public static class Product {

    @Id
    private Integer id;

    private String name;

    @Lob
    private Clob warranty;

    //Getters and setters are omitted for brevity

}
```
保存Clob类型：
```
String warranty = "My product warranty";
final Product product = new Product();
product.setId( 1 );
product.setName( "Mobile phone" );
product.setWarranty( ClobProxy.generateProxy( warranty ) );
entityManager.persist( product );
```
读取Clob: `Reader reader = product.getWarranty().getCharacterStream()`

#### 1.3.3.2. 映射BLOB ####
和CLOB类似，使用Blob类型映射（或byte[]）。使用BlobProxy存储。

### 1.3.4. 时间类型的映射 ###
SQL定义了三种标准日期类型：1. DATE， 2. TIME， 3. TIMESTAMP
@Temporal注解表名sql时间映射类型，使用 java.util.Date （java.util.Calendar也可以）来映射事件类型：
1. 映射成 DATE（年月日）：
   ```
    @Column(name = "`time`")
	@Temporal(TemporalType.DATE)
	private Date time;
   ```
2. 映射成 TIME（时分秒）：
   ```
    @Column(name = "`time`")
    @Temporal(TemporalType.TIME)
    private Date time;
   ```
2. 映射成 TIME（年月日 + 时分秒）：
   ```
    @Column(name = "`time`")
    @Temporal(TemporalType.TIMESTAMP)
    private Date time;
   ```
如果未指定时区，则JDBC驱动程序将使用底层的JVM默认时区。

### 1.3.5. @Generated ###
使用@Generated注解是为了使Hibernate在持久化或更新实体后，可以获取当前注解的属性。@Generated注解接受一个GenerationTime枚举值指定数据库什么时候应该生成该属性的值。
1. NEVER （默认），给定的属性值不在数据库内生成。

2. INSERT，给定的属性值在插入时生成，但在后续更新中不会重新生成。诸如creationTimestamp之类的属性都属于此类。

3. ALWAYS，属性值是在插入和更新时生成的。

示例：
```
@Entity(name = "Person")
public static class Person {

	@Id
	private Long id;
	private String firstName;
	private String middleName;
	private String lastName;

	@Generated( value = GenerationTime.ALWAYS )
	@Column(columnDefinition =
		"AS CONCAT(" +
		"	COALESCE(firstName, ''), " +
		"	COALESCE(' ' + middleName, ''), " +
		"	COALESCE(' ' + lastName, '') " +
		")")
	private String fullName;
}
```
### 1.3.6. @GeneratorType ###
@GeneratorType 用自定义类来处理被注解属性的值。接受两个参数 `@GeneratorType( type = ?.class, when = GenerationTime.?)`。
```
public static class CurrentUser {

	public static final CurrentUser INSTANCE = new CurrentUser();

	private static final ThreadLocal<String> storage = new ThreadLocal<>();

	public void logIn(String user) {
		storage.set( user );
	}

	public void logOut() {
		storage.remove();
	}

	public String get() {
		return storage.get();
	}
}

public static class LoggedUserGenerator implements ValueGenerator<String> {

	@Override
	public String generateValue(
			Session session, Object owner) {
		return CurrentUser.INSTANCE.get();
	}
}

@Entity(name = "Person")
public static class Person {

	@Id
	private Long id;

	private String firstName;

	private String lastName;

	@GeneratorType( type = LoggedUserGenerator.class, when = GenerationTime.INSERT)
	private String createdBy;

	@GeneratorType( type = LoggedUserGenerator.class, when = GenerationTime.ALWAYS)
	private String updatedBy;

}
```

### 1.3.7. @ValueGenerationType ###
@ValueGenerationType 这是一种声明生成的属性或定制生成器的新方法，可以自己定义如何生成值，也可以利用数据库生成值。需要把实际的生成逻辑添加到实现AnnotationValueGeneration接口的类中。

```
@Entity(name = "Event")
public static class Event {

	@Id
	@GeneratedValue
	private Long id;

	@Column(name = "`timestamp`")
	@FunctionCreationTimestamp
	private Date timestamp;

	//Constructors, getters, and setters are omitted for brevity
}

@ValueGenerationType(generatedBy = FunctionCreationValueGeneration.class)
@Retention(RetentionPolicy.RUNTIME)
public @interface FunctionCreationTimestamp {}

public static class FunctionCreationValueGeneration
		implements AnnotationValueGeneration<FunctionCreationTimestamp> {

	@Override
	public void initialize(FunctionCreationTimestamp annotation, Class<?> propertyType) {
	}

	/**
	 * Generate value on INSERT
	 * @return when to generate the value
	 */
	public GenerationTiming getGenerationTiming() {
		return GenerationTiming.INSERT;
	}

	/**
	 * Returns null because the value is generated by the database.
	 * @return null
	 */
	public ValueGenerator<?> getValueGenerator() {
		return null;
	}

	/**
	 * Returns true because the value is generated by the database.
	 * @return true
	 */
	public boolean referenceColumnInSql() {
        // 表明使用 getDatabaseGeneratedReferencedColumnValue() 方法的值
		return true;
	}

	/**
	 * Returns the database-generated value
	 * @return database-generated value
	 */
	public String getDatabaseGeneratedReferencedColumnValue() {
		return "current_timestamp";
	}
}
```

如果要在程序里自动生成值，即getValueGenerator， 参考 GeneratedValueGeneration 和 UpdateTimestampGeneration 的实现，也可以自定义一个 ValueGenerator

### 1.3.8. 列转换器 ###
@Columnns 指定该属性可以用于多个列。
@ColumnTransformer 为指定列进行数据处理

```
@Columns(columns = {
    @Column(name = "money"),
    @Column(name = "currency")
})
@ColumnTransformer(
    forColumn = "money",
    read = "money / 100",
    write = "? * 100"
)
private MonetaryAmount wallet;
```

**@Formula**
指定该变量值由其他属性值计算得来。

```
private Double credit;

private Double rate;

@Formula(value = "credit * rate")
private Double interest;
```


### 1.3.9. 保留字 ###
保留字在用作列名表名时需要加上引号。
```
@Column(name = "\"name\"")
private String name;
```
也可以配置 globally_quoted_identifiers 属性，设置全局保留字的引号处理。
```
<property
    name="hibernate.globally_quoted_identifiers"
    value="true"
/>
```

## 1.4. 可嵌入类型 ##
略。

## 1.5. 实体类型 ##
>JPA 2.1规范的第2.1节“实体类” 定义了其对实体类的要求。希望在JPA提供者之间保持可移植性的应用程序应遵守以下要求：
>- 实体类必须带有javax.persistence.Entity注解（或在XML映射中这样表示）。
>- 实体类必须具有公共或受保护的无参数构造函数。它还可以定义其他构造函数。
>- 实体类必须是顶级类。
>- 枚举或接口不能指定为实体。
>- 实体类不能 final。实体类的任何方法或持久实例变量都不得为 final。
>- 如果实体实例要作为分离对象远程使用，则实体类必须实现该Serializable接口。
>- 抽象类和具体类都可以是实体。实体可以扩展非实体类以及实体类，并且非实体类可以扩展实体类。
>- 实体的持久状态由实例变量表示，实例变量可以对应于JavaBean样式的属性。实例变量只能由实体实例本身直接从实体的方法内部访问。客户只能通过实体的访问器方法（getter / setter方法）或其他业务方法来使用实体的状态。

但是，Hibernate的要求并不严格。与上面列表的区别包括：
>- 实体类必须具有无参数构造函数，该构造函数可以是公共的，受保护的或程序包可见性。它还可以定义其他构造函数。
>- 实体类不必是顶级类。
>- 从技术上讲，Hibernate可以持久化最终类或具有最终持久性状态访问器（getter / setter）方法的类。但是，通常不是一个好主意，因为这样做会阻止Hibernate生成用于延迟加载实体的代理。
>- Hibernate并不限制应用程序开发人员公开实例变量并从实体类本身之外引用它们。然而，这种范例的有效性是有争议的。

### 1.5.1. final 类 ###
Hibernate的主要功能是可以通过运行时代理延迟加载某些实体实例变量（属性）。此功能取决于实体类是否为非最终类，或者取决于实现声明所有属性获取器/设置器的接口。为了性能调整，应该避免将持久属性以及对应的 getter 和 setter 声明为 final。

### 1.5.2. 无参构造函数 ###
实体类应具有无参数的构造函数。在Hibernate里，如果您希望利用运行时代理生成即使用懒加载，则应至少使用包可见性来定义构造函数。

### 1.5.3. 声明持久性属性的获取器和设置器 ###
Hibernate 建议遵循 JavaBean 约定并为实体持久属性定义 getter 和 setter。同样，如果希望使用运行时代理生成进行延迟加载，则 getter / setter 方法应至少授予对程序包可见性（default）的访问权限。

### 1.5.4. 映射实体 ###
映射实体的主要部分是javax.persistence.Entity注解。
该@Entity注解定义只是name它是用来给一个特定的实体名称为JPQL查询中使用的属性。缺少 name 则使用隐式命名规则。

一般实体名就是表名，如果要显示指明表名和实体名的关系，可以使用@Table和@Entity 一起注解来说明：
```
@Entity(name = "Book")
@Table(
        catalog = "public",
        schema = "store",
        name = "book"
)
public static class Book {

    @Id
    private Long id;

    private String title;

    private String author;

    //Getters and setters are omitted for brevity
}
```

### 1.5.5. 实现持久化对象的 equals() hashCode() ###
对于同类型持久化对象，要判断是否相等，在持久化方面应该用主键信息去比较。而Java的默认情况下使用内存地址来判断，所以在使用Hibernate，用到 Set 存储关联实体时，或者有比较实体是否相等时应该考虑实现equals() hashCode()方法。
```
@Entity(name = "Book")
public static class Book {

	@Id
	@GeneratedValue
	private Long id;
	private String title;
	private String author;
	@NaturalId
	private String isbn;

	//Getters and setters are omitted for brevity

	@Override
	public boolean equals(Object o) {
		if ( this == o ) {
			return true;
		}
		if ( o == null || getClass() != o.getClass() ) {
			return false;
		}
		Book book = (Book) o;
		return Objects.equals( isbn, book.isbn );
	}

	@Override
	public int hashCode() {
		return Objects.hash( isbn );
	}
}
```

### 1.5.6. 实体映射到 SQL 查询 ###
可以使用@Subselect注解将实体映射到SQL查询。AccountSummary实体映射中@Synchronize注解的目的是指示Hibernate基础@SubselectSQL查询需要哪些数据库表
```
@Entity(name = "Client")
@Table(name = "client")
public static class Client {

	@Id
	private Long id;

	@Column(name = "first_name")
	private String firstName;

	@Column(name = "last_name")
	private String lastName;

	//Getters and setters omitted for brevity

}

@Entity(name = "Account")
@Table(name = "account")
public static class Account {

	@Id
	private Long id;

	@ManyToOne
	private Client client;

	private String description;

	//Getters and setters omitted for brevity

}

@Entity(name = "AccountTransaction")
@Table(name = "account_transaction")
public static class AccountTransaction {

	@Id
	@GeneratedValue
	private Long id;

	@ManyToOne
	private Account account;

	private Integer cents;

	private String description;

	//Getters and setters omitted for brevity

}

@Entity(name = "AccountSummary")
@Subselect(
	"select " +
	"	a.id as id, " +
	"	concat(concat(c.first_name, ' '), c.last_name) as clientName, " +
	"	sum(at.cents) as balance " +
	"from account a " +
	"join client c on c.id = a.client_id " +
	"join account_transaction at on a.id = at.account_id " +
	"group by a.id, concat(concat(c.first_name, ' '), c.last_name)"
)
@Synchronize( {"client", "account", "account_transaction"} )
public static class AccountSummary {

	@Id
	private Long id;

	private String clientName;

	private int balance;

	//Getters and setters omitted for brevity

}
```
用 entityManager 可以直接取到该实体类，但是该类只是类似数据库的视图，没有实际存储：
```
AccountSummary summary = entityManager.find( AccountSummary.class, 1L );
```
当有新的 AccountTransaction 存到数据库时，使用下面的 flush 和 refresh 方法就可以直接刷新AccountSummary里的值。
```
AccountTransaction transaction = new AccountTransaction();
transaction.setAccount( entityManager.getReference( Account.class, 1L ) );
transaction.setDescription( "Shopping" );
transaction.setCents( -100 * 2200 );
entityManager.persist( transaction );
entityManager.flush();
entityManager.refresh( summary );
```

### 1.5.7. 自定义代理类 ###
当实体类被标识为 final 后，Hibernate 将不会自动生成代理类，这种情况下可以使用 @Proxy 指定代理类, 当使用session.getReference 或者 session.load 方法时，hibernate就会返回代理类：
```
public interface Identifiable {

	Long getId();

	void setId(Long id);
}

@Entity( name = "Book" )
@Proxy(proxyClass = Identifiable.class)
public static final class Book implements Identifiable {

	@Id
	private Long id;

	private String title;

	private String author;

	@Override
	public Long getId() {
		return id;
	}

	@Override
	public void setId(Long id) {
		this.id = id;
	}

	//Other getters and setters omitted for brevity
}
```

### 1.5.8. 动态代理 ###
略。

### 1.5.9. 自定义持久化类 ###
该@Persister注释用于指定自定义实体或集合的持久化逻辑。

对于实体，自定义持久化类必须实现EntityPersister接口。
对于集合，自定义持久化类必须实现CollectionPersister接口。

```
@Entity
@Persister( impl = EntityPersister.class )
public class Author {

    @Id
    public Integer id;

    @OneToMany( mappedBy = "author" )
    @Persister( impl = CollectionPersister.class )
    public Set<Book> books = new HashSet<>();

    //Getters and setters omitted for brevity

    public void addBook(Book book) {
        this.books.add( book );
        book.setAuthor( this );
    }
}

@Entity
@Persister( impl = EntityPersister.class )
public class Book {

    @Id
    public Integer id;

    private String title;

    @ManyToOne(fetch = FetchType.LAZY)
    public Author author;

    //Getters and setters omitted for brevity
}
```

### 1.5.10. 访问策略 ###
略。

## 1.6. 标识符 ##
标识符为实体的主键。它们用于唯一地标识每个特定实体。

Hibernate 和 JPA 都对相应的数据库列进行以下假设：
- UNIQUE 这些值必须唯一地标识每一行。
- NOT NULL 该值不能为空。对于复合ID，任何部分都不能为null。
- IMMUTABLE 这些值一旦插入，就无法更改。

### 1.6.1. 简单标识符 ###
简单标识符映射到单个基本属性，并使用javax.persistence.Id注释表示。

根据JPA，只能将以下类型用作标识符属性类型：
- 任何Java基本类型
- 任何原始包装器类型
- java.lang.String
- java.util.Date （TemporalType＃DATE）
- java.sql.Date
- java.math.BigDecimal
- java.math.BigInteger

### 1.6.2. 复合标识符 ###
复合标识符对应于一个或多个持久属性。JPA规范定义的管理组合标识符的规则：
- 复合标识符必须由“主键类”表示。主键类可以使用javax.persistence.EmbeddedId批注定义，也可以使用批注定义javax.persistence.IdClass。
- 主键类必须是公共的，并且必须具有公共的无参数构造函数。
- 主键类必须可序列化。
- 主键类必须定义equals和hashCode方法，该方法与主键映射到的基础数据库类型的相等性一致。

Hibernate确实允许通过多个@Id属性在没有“主键类”的情况下定义组合标识符。

### 1.6.3. @EmbeddedId ###
简单例子
```
@Entity(name = "SystemUser")
public static class SystemUser {

	@EmbeddedId
	private PK pk;

	private String name;

	//Getters and setters are omitted for brevity
}

@Embeddable
public static class PK implements Serializable {

	private String subsystem;

	private String username;

	public PK(String subsystem, String username) {
		this.subsystem = subsystem;
		this.username = username;
	}

	private PK() {
	}

	@Override
	public boolean equals(Object o) {
		if ( this == o ) {
			return true;
		}
		if ( o == null || getClass() != o.getClass() ) {
			return false;
		}
		PK pk = (PK) o;
		return Objects.equals( subsystem, pk.subsystem ) &&
				Objects.equals( username, pk.username );
	}

	@Override
	public int hashCode() {
		return Objects.hash( subsystem, username );
	}
}
```
在使用外建作为主键的情况，还可以在主键类里使用 @ManytoOne：
```
@Entity(name = "SystemUser")
public static class SystemUser {

	@EmbeddedId
	private PK pk;

	private String name;

	//Getters and setters are omitted for brevity
}

@Entity(name = "Subsystem")
public static class Subsystem {

	@Id
	private String id;

	private String description;

	//Getters and setters are omitted for brevity
}

@Embeddable
public static class PK implements Serializable {

	@ManyToOne(fetch = FetchType.LAZY)
	private Subsystem subsystem;

	private String username;

	public PK(Subsystem subsystem, String username) {
		this.subsystem = subsystem;
		this.username = username;
	}

	private PK() {
	}

	@Override
	public boolean equals(Object o) {
		if ( this == o ) {
			return true;
		}
		if ( o == null || getClass() != o.getClass() ) {
			return false;
		}
		PK pk = (PK) o;
		return Objects.equals( subsystem, pk.subsystem ) &&
				Objects.equals( username, pk.username );
	}

	@Override
	public int hashCode() {
		return Objects.hash( subsystem, username );
	}
}
```

### 1.6.4. @IdClass ###
和 @EmbededId 的区别主要是在实体类里会定义组成标识符的各个属性，而@IdClass 指向的类只是个“影子”。 一样可以加@ManytoOne。
```
@Entity(name = "SystemUser")
@IdClass( PK.class )
public static class SystemUser {

	@Id
	private String subsystem;

	@Id
	private String username;

	private String name;

	public PK getId() {
		return new PK(
			subsystem,
			username
		);
	}

	public void setId(PK id) {
		this.subsystem = id.getSubsystem();
		this.username = id.getUsername();
	}

	//Getters and setters are omitted for brevity
}

public static class PK implements Serializable {

	private String subsystem;

	private String username;

	public PK(String subsystem, String username) {
		this.subsystem = subsystem;
		this.username = username;
	}

	private PK() {
	}

	//Getters and setters are omitted for brevity

	@Override
	public boolean equals(Object o) {
		if ( this == o ) {
			return true;
		}
		if ( o == null || getClass() != o.getClass() ) {
			return false;
		}
		PK pk = (PK) o;
		return Objects.equals( subsystem, pk.subsystem ) &&
				Objects.equals( username, pk.username );
	}

	@Override
	public int hashCode() {
		return Objects.hash( subsystem, username );
	}
}
```

### 1.6.5. 具有关联的复合标识符 ###
Hibernate 允许使用关联关系的实体作为主键。PersonAddress实体标识符由两个@ManyToOne关联组成。
```
@Entity(name = "Book")
public static class Book implements Serializable {

	@Id
	@ManyToOne(fetch = FetchType.LAZY)
	private Author author;

	@Id
	@ManyToOne(fetch = FetchType.LAZY)
	private Publisher publisher;

	@Id
	private String title;

	public Book(Author author, Publisher publisher, String title) {
		this.author = author;
		this.publisher = publisher;
		this.title = title;
	}

	private Book() {
	}

	//Getters and setters are omitted for brevity


	@Override
	public boolean equals(Object o) {
		if ( this == o ) {
			return true;
		}
		if ( o == null || getClass() != o.getClass() ) {
			return false;
		}
		Book book = (Book) o;
		return Objects.equals( author, book.author ) &&
				Objects.equals( publisher, book.publisher ) &&
				Objects.equals( title, book.title );
	}

	@Override
	public int hashCode() {
		return Objects.hash( author, publisher, title );
	}
}
```

在 Hibernate 查询时使用如下语句：
```
Book book = entityManager.find( Book.class, new Book(
	author,
	publisher,
	"High-Performance Java Persistence"
) );
```

### 1.6.6. 标识符和自动生成属性 ###
Hibernate 不允许标识符带有 @Generated, @CreationTimestamp or @ValueGenerationType 之类的自动生成注解。应该在持久化前，在代码直接访问标识符属性赋值。

### 1.6.7. 生成标识符的值 ###
标识符值的生成使用 @javax.persistence.GeneratedValue 注解指示。而如何生成标识符的值则是用 GenerationType 来表示：
- AUTO（默认）把主键生成策略交给持久化引擎。

- IDENTITY 此种主键生成策略就是通常所说的主键自增长,数据库在插入数据时,会自动给主键赋值,比如MYSQL可以在创建表时声明"auto_increment" 来指定主键自增长。

- SEQUENCE  在某些数据库中,不支持主键自增长,比如Oracle,其提供了一种叫做"序列(sequence)"的机制生成主键。此时,GenerationType.SEQUENCE就可以作为主键生成策略。该策略一般与另外一个注解一起使用@SequenceGenerator,@SequenceGenerator注解指定了生成主键的序列.然后JPA会根据注解内容创建一个序列(或使用一个现有的序列)。如果不指定序列,则会自动生成一个序列 hibernate_sequence。
```
@Entity(name = "Product")
public static class Product {

	@Id
	@GeneratedValue(
		strategy = GenerationType.SEQUENCE,
		generator = "sequence-generator"
	)
	@SequenceGenerator(
		name = "sequence-generator",
		sequenceName = "product_sequence"
	)
	private Long id;

	@Column(name = "product_name")
	private String name;

	//Getters and setters are omitted for brevity

}
```


- TABLE 使用一个特定的数据库表格来保存主键,持久化引擎通过关系数据库的一张特定的表格来生成主键,这种策略的好处就是不依赖于外部环境和数据库的具体实现,在不同数据库间可以很容易的进行移植,但由于其不能充分利用数据库的特性,所以不会优先使用。该策略一般与另外一个注解一起使用@TableGenerator,@TableGenerator注解指定了生成主键的表。如果不指定序列表,则会生成一张默认的序列表,表中的列名也是自动生成,数据库上会生成一张作为sequence的表。
```
create table hibernate_sequences (
    sequence_name varchar2(255 char) not null,
    next_val number(19,0),
    primary key (sequence_name)
)
```

### 1.6.8. UUID 生成器 ###

Hibernate支持UUID标识符值生成。这是通过其org.hibernate.id.UUIDGenerator 支持的。
使用默认 UUID 生成策略：
```
@Entity(name = "Book")
public static class Book {

	@Id
	@GeneratedValue
	private UUID id;

	private String title;

	private String author;

	//Getters and setters are omitted for brevity
}
```


指定策略：
```
@Entity(name = "Book")
public static class Book {

	@Id
	@GeneratedValue( generator = "custom-uuid" )
	@GenericGenerator(
		name = "custom-uuid",
		strategy = "org.hibernate.id.UUIDGenerator",
		parameters = {
			@Parameter(
				name = "uuid_gen_strategy_class",
				value = "org.hibernate.id.uuid.CustomVersionOneStrategy"
			)
		}
	)
	private UUID id;

	private String title;

	private String author;

	//Getters and setters are omitted for brevity
}
```

### 1.6.9. 标识符优化器 ###
Needed.

### 1.6.10. @GenericGenerator ###

@GenericGenerator允许集成任何Hibernate org.hibernate.id.IdentifierGenerator实现。要使用池式优化器，实体映射必须使用@GenericGenerator注释：
```
@Entity(name = "Product")
public static class Product {

	@Id
	@GeneratedValue(
		strategy = GenerationType.SEQUENCE,
		generator = "product_generator"
	)
	@GenericGenerator(
		name = "product_generator",
		strategy = "org.hibernate.id.enhanced.SequenceStyleGenerator",
		parameters = {
			@Parameter(name = "sequence_name", value = "product_sequence"),
			@Parameter(name = "initial_value", value = "1"),
			@Parameter(name = "increment_size", value = "3"),
			@Parameter(name = "optimizer", value = "pooled-lo")
		}
	)
	private Long id;

	@Column(name = "p_name")
	private String name;

	@Column(name = "p_number")
	private String number;

	//Getters and setters are omitted for brevity

}
```

### 1.6.11. 派生标识符 ###
如果两个实体的关系是一对一的关系，则他们的主键标识符可以一样。可以使用 @MapsId / @PrimaryKeyJoinColumn 同步两个实体的标识符：
```
@Entity(name = "Person")
public static class Person  {

	@Id
	private Long id;

	@NaturalId
	private String registrationNumber;

	public Person() {}

	public Person(String registrationNumber) {
		this.registrationNumber = registrationNumber;
	}

	//Getters and setters are omitted for brevity
}

@Entity(name = "PersonDetails")
public static class PersonDetails  {

	@Id
	private Long id;

	private String nickName;

	@OneToOne
	@MapsId
	private Person person;

	//Getters and setters are omitted for brevity
}
```
PersonDetails实体标识符的值是从其父Person实体的标识符“派生”的。存储时直接用Person的id值：
```
doInJPA( this::entityManagerFactory, entityManager -> {
	Person person = new Person( "ABC-123" );
	person.setId( 1L );
	entityManager.persist( person );
	PersonDetails personDetails = new PersonDetails();
	personDetails.setNickName( "John Doe" );
	personDetails.setPerson( person );
	entityManager.persist( personDetails );
} );

doInJPA( this::entityManagerFactory, entityManager -> {
	PersonDetails personDetails = entityManager.find( PersonDetails.class, 1L );
	assertEquals("John Doe", personDetails.getNickName());
} );
```
这个方式也可以用在复合标识符 @EmbeddedId 的情况。

### 1.6.12. @RowId ###
@RowId 可以注解实体类，用Hibnernate 从数据库取出持久化类后，进行crud操作时，hibernate 会使用一个rowid作为条件操作数据库。
```
@Entity(name = "Product")
@RowId("ROWID")
public static class Product {

	@Id
	private Long id;

	@Column(name = "`name`")
	private String name;

	@Column(name = "`number`")
	private String number;

	//Getters and setters are omitted for brevity

}
```

```
Product product = entityManager.find( Product.class, 1L );

product.setName( "Smart phone" );
SELECT
    p.id as id1_0_0_,
    p."name" as name2_0_0_,
    p."number" as number3_0_0_,
    p.ROWID as rowid_0_
FROM
    Product p
WHERE
    p.id = ?

-- binding parameter [1] as [BIGINT] - [1]

-- extracted value ([name2_0_0_] : [VARCHAR]) - [Mobile phone]
-- extracted value ([number3_0_0_] : [VARCHAR]) - [123-456-7890]
-- extracted ROWID value: AAAwkBAAEAAACP3AAA

UPDATE
    Product
SET
    "name" = ?,
    "number" = ?
WHERE
    ROWID = ?

-- binding parameter [1] as [VARCHAR] - [Smart phone]
-- binding parameter [2] as [VARCHAR] - [123-456-7890]
-- binding parameter [3] as ROWID     - [AAAwkBAAEAAACP3AAA]
```

## 1.7. 关联关系 ##

### 1.7.1. @ManyToOne ###
@ManyToOne 是最常见的关联，在关系数据库中也具有直接等效项（例如，外键），因此它在子实体和父实体之间建立了关系。这里直接用 @JoinColume 创建外键。@JoinColumn所在实体是关系拥有方。
```
@Entity(name = "Person")
public static class Person {

	@Id
	@GeneratedValue
	private Long id;

	//Getters and setters are omitted for brevity

}

@Entity(name = "Phone")
public static class Phone {

	@Id
	@GeneratedValue
	private Long id;

	@Column(name = "`number`")
	private String number;

	@ManyToOne
	@JoinColumn(name = "person_id",
			foreignKey = @ForeignKey(name = "PERSON_ID_FK")
	)
	private Person person;

	//Getters and setters are omitted for brevity

}
```

### 1.7.2. @OneToMany ###

#### 1.7.2.1. 单向 ####
一对多关联关系。如果一方有 @OneToMany 的注解，而多的一方没有 @ManyToOne，则这样的关联关系是单向的。单向一对多关联关系，需要一张独立的表来负责维护。在删除子实体时，单向关联不是非常有效。
```
@Entity(name = "Person")
public static class Person {

	@Id
	@GeneratedValue
	private Long id;

	@OneToMany(cascade = CascadeType.ALL, orphanRemoval = true)
	private List<Phone> phones = new ArrayList<>();

	//Getters and setters are omitted for brevity

}

@Entity(name = "Phone")
public static class Phone {

	@Id
	@GeneratedValue
	private Long id;

	@Column(name = "`number`")
	private String number;

	//Getters and setters are omitted for brevity

}
```
以上的实体类结构所对应的数据库结构：
```
CREATE TABLE Person (
    id BIGINT NOT NULL ,
    PRIMARY KEY ( id )
)

CREATE TABLE Person_Phone (
    Person_id BIGINT NOT NULL ,
    phones_id BIGINT NOT NULL
)

CREATE TABLE Phone (
    id BIGINT NOT NULL ,
    number VARCHAR(255) ,
    PRIMARY KEY ( id )
)

ALTER TABLE Person_Phone
ADD CONSTRAINT UK_9uhc5itwc9h5gcng944pcaslf
UNIQUE (phones_id)

ALTER TABLE Person_Phone
ADD CONSTRAINT FKr38us2n8g5p9rj0b494sd3391
FOREIGN KEY (phones_id) REFERENCES Phone

ALTER TABLE Person_Phone
ADD CONSTRAINT FK2ex4e4p7w1cj310kg2woisjl2
FOREIGN KEY (Person_id) REFERENCES Person
```

#### 1.7.2.2. 双向 ####
双向一对多关联关系需要在多方加上一方的引用，并且加上 @ManyToOne。这样双方都能取到对方的数据，而为了不再生成中间表，需要把创建外键的任务交给多的一方，在 @OneToMany 里加上 mappedBy 属性，值为一方在多方里的引用变量名称，在这个例子中就是 “person” 。

orphanRemoval的作用是某个 phone 不在被父实体引用后，是否将其在Phone表中删除，如果标为false，则只是将Phone表中相应记录的外键值设置为 Null。
```
@Entity(name = "Person")
public static class Person {

	@Id
	@GeneratedValue
	private Long id;

	@OneToMany(mappedBy = "person", cascade = CascadeType.ALL, orphanRemoval = true)
	private List<Phone> phones = new ArrayList<>();

	//Getters and setters are omitted for brevity

	public void addPhone(Phone phone) {
		phones.add( phone );
		phone.setPerson( this );
	}

	public void removePhone(Phone phone) {
		phones.remove( phone );
		phone.setPerson( null );
	}
}

@Entity(name = "Phone")
public static class Phone {

	@Id
	@GeneratedValue
	private Long id;

	@NaturalId
	@Column(name = "`number`", unique = true)
	private String number;

	@ManyToOne
	private Person person;

	//Getters and setters are omitted for brevity

	@Override
	public boolean equals(Object o) {
		if ( this == o ) {
			return true;
		}
		if ( o == null || getClass() != o.getClass() ) {
			return false;
		}
		Phone phone = (Phone) o;
		return Objects.equals( number, phone.number );
	}

	@Override
	public int hashCode() {
		return Objects.hash( number );
	}
}
```
双向一对多的数据库结构：
```
CREATE TABLE Person (
    id BIGINT NOT NULL ,
    PRIMARY KEY ( id )
)

CREATE TABLE Phone (
    id BIGINT NOT NULL ,
    number VARCHAR(255) ,
    person_id BIGINT ,
    PRIMARY KEY ( id )
)

ALTER TABLE Phone
ADD CONSTRAINT UK_l329ab0g4c1t78onljnxmbnp6
UNIQUE (number)

ALTER TABLE Phone
ADD CONSTRAINT FKmw13yfsjypiiq0i1osdkaeqpg
FOREIGN KEY (person_id) REFERENCES Person
```


### 1.7.3. @OneToOne ###

#### 1.7.3.1. 单向 ####
单向一对一关系，只要在其中一方的实体内加上 @OneToOne 负责关联关系和 @JoinColume 来创建外键。
```
@Entity(name = "Phone")
public static class Phone {

	@Id
	@GeneratedValue
	private Long id;

	@Column(name = "`number`")
	private String number;

	@OneToOne
	@JoinColumn(name = "details_id")
	private PhoneDetails details;

	//Getters and setters are omitted for brevity

}

@Entity(name = "PhoneDetails")
public static class PhoneDetails {

	@Id
	@GeneratedValue
	private Long id;

	private String provider;

	private String technology;

	//Getters and setters are omitted for brevity

}
```
数据库结构：
```
CREATE TABLE Phone (
    id BIGINT NOT NULL ,
    number VARCHAR(255) ,
    details_id BIGINT ,
    PRIMARY KEY ( id )
)

CREATE TABLE PhoneDetails (
    id BIGINT NOT NULL ,
    provider VARCHAR(255) ,
    technology VARCHAR(255) ,
    PRIMARY KEY ( id )
)

ALTER TABLE Phone
ADD CONSTRAINT FKnoj7cj83ppfqbnvqqa5kolub7
FOREIGN KEY (details_id) REFERENCES PhoneDetails
```

#### 1.7.3.2. 双向 ####
双向一对一关系需要在两方都加上对方的引用并加上 @OneToOne 注解。

在这里可以在其中一方加上 mappedBy 属性避免生成两个外键。一般来说，应该在父实体类上加上 mappedBy，让子实体类的生命周期跟随父实体类，生成外键维护关系。例如 Phone 和 PhoneDetails，Phone应为父实体类，因为现有 Phone 才有 PhoneDetails。删除Phone的记录时，级联删除 PhoneDetails（CascadeType.ALL）。

```
@Entity(name = "Phone")
public static class Phone {

	@Id
	@GeneratedValue
	private Long id;

	@Column(name = "`number`")
	private String number;

	@OneToOne(
		mappedBy = "phone",
		cascade = CascadeType.ALL,
		orphanRemoval = true,
		fetch = FetchType.LAZY
	)
	private PhoneDetails details;

	//Getters and setters are omitted for brevity

	public void addDetails(PhoneDetails details) {
		details.setPhone( this );
		this.details = details;
	}

	public void removeDetails() {
		if ( details != null ) {
			details.setPhone( null );
			this.details = null;
		}
	}
}

@Entity(name = "PhoneDetails")
public static class PhoneDetails {

	@Id
	@GeneratedValue
	private Long id;

	private String provider;

	private String technology;

	@OneToOne(fetch = FetchType.LAZY)
	@JoinColumn(name = "phone_id")
	private Phone phone;

	//Getters and setters are omitted for brevity

}
```

### 1.7.4. @ManyToMany ###

#### 1.7.4.1. 单向 ####
在单向多对多的关系，级联操作不要包括删除，因为 Address 有可能被两个 Person 引用。

和 @OneToMany 一样，在单向关系中，把多方的某些元素删除时，Hibernate 需要把中间表中多方的关联元素删除，在把没指定删除的元素在加回去。
```
@Entity(name = "Person")
public static class Person {

	@Id
	@GeneratedValue
	private Long id;

	@ManyToMany(cascade = {CascadeType.PERSIST, CascadeType.MERGE})
	private List<Address> addresses = new ArrayList<>();

	//Getters and setters are omitted for brevity

}

@Entity(name = "Address")
public static class Address {

	@Id
	@GeneratedValue
	private Long id;

	private String street;

	@Column(name = "`number`")
	private String number;

	//Getters and setters are omitted for brevity

}
```

数据库：
```
CREATE TABLE Address (
    id BIGINT NOT NULL ,
    number VARCHAR(255) ,
    street VARCHAR(255) ,
    PRIMARY KEY ( id )
)

CREATE TABLE Person (
    id BIGINT NOT NULL ,
    PRIMARY KEY ( id )
)

CREATE TABLE Person_Address (
    Person_id BIGINT NOT NULL ,
    addresses_id BIGINT NOT NULL
)
```

#### 1.7.4.2. 无链接实体的双向 ####

双向ManyToMany时依赖的依然是mappedBy。在两个实体中都需要注解@ManyToMany，但是因为是多对多，需要依赖虚拟的中间表，在下面的例子中在 Address 中标注 mappedBy，让Person管理这张表（Person_Address），否则会生成另一张中间表（Address_Person）。在Person类的addAddress() 和 removeAddress()中会有对Address的操作。

但是这样的双向关系并没有像双向一对多那样对删除操作有优化作用，因为外键不受控制，需要经过删除所有记录，在把没指定删除的加回来。
```
@Entity(name = "Person")
public static class Person {

	@Id
	@GeneratedValue
	private Long id;

	@NaturalId
	private String registrationNumber;

	@ManyToMany(cascade = {CascadeType.PERSIST, CascadeType.MERGE})
	private List<Address> addresses = new ArrayList<>();

	//Getters and setters are omitted for brevity

	public void addAddress(Address address) {
		addresses.add( address );
		address.getOwners().add( this );
	}

	public void removeAddress(Address address) {
		addresses.remove( address );
		address.getOwners().remove( this );
	}

	@Override
	public boolean equals(Object o) {
		if ( this == o ) {
			return true;
		}
		if ( o == null || getClass() != o.getClass() ) {
			return false;
		}
		Person person = (Person) o;
		return Objects.equals( registrationNumber, person.registrationNumber );
	}

	@Override
	public int hashCode() {
		return Objects.hash( registrationNumber );
	}
}

@Entity(name = "Address")
public static class Address {

	@Id
	@GeneratedValue
	private Long id;

	private String street;

	@Column(name = "`number`")
	private String number;

	private String postalCode;

	@ManyToMany(mappedBy = "addresses")
	private List<Person> owners = new ArrayList<>();

	//Getters and setters are omitted for brevity

	@Override
	public boolean equals(Object o) {
		if ( this == o ) {
			return true;
		}
		if ( o == null || getClass() != o.getClass() ) {
			return false;
		}
		Address address = (Address) o;
		return Objects.equals( street, address.street ) &&
				Objects.equals( number, address.number ) &&
				Objects.equals( postalCode, address.postalCode );
	}

	@Override
	public int hashCode() {
		return Objects.hash( street, number, postalCode );
	}
}
```

数据库结构：
```
CREATE TABLE Address (
    id BIGINT NOT NULL ,
    number VARCHAR(255) ,
    postalCode VARCHAR(255) ,
    street VARCHAR(255) ,
    PRIMARY KEY ( id )
)

CREATE TABLE Person (
    id BIGINT NOT NULL ,
    registrationNumber VARCHAR(255) ,
    PRIMARY KEY ( id )
)

CREATE TABLE Person_Address (
    owners_id BIGINT NOT NULL ,
    addresses_id BIGINT NOT NULL
)
```

#### 1.7.4.3. 有链接实体的双向 ####
为了多对多关系也能对删除操作有优化，必须直接公开链接表并将 @ManyToMany 关联分为两个双向 @OneToMany 关系。
```
@Entity(name = "Person")
public static class Person implements Serializable {

	@Id
	@GeneratedValue
	private Long id;

	@NaturalId
	private String registrationNumber;

	@OneToMany(
		mappedBy = "person",
		cascade = CascadeType.ALL,
		orphanRemoval = true
	)
	private List<PersonAddress> addresses = new ArrayList<>();

	//Getters and setters are omitted for brevity

	public void addAddress(Address address) {
		PersonAddress personAddress = new PersonAddress( this, address );
		addresses.add( personAddress );
		address.getOwners().add( personAddress );
	}

	public void removeAddress(Address address) {
		PersonAddress personAddress = new PersonAddress( this, address );
		address.getOwners().remove( personAddress );
		addresses.remove( personAddress );
		personAddress.setPerson( null );
		personAddress.setAddress( null );
	}

	@Override
	public boolean equals(Object o) {
		if ( this == o ) {
			return true;
		}
		if ( o == null || getClass() != o.getClass() ) {
			return false;
		}
		Person person = (Person) o;
		return Objects.equals( registrationNumber, person.registrationNumber );
	}

	@Override
	public int hashCode() {
		return Objects.hash( registrationNumber );
	}
}

@Entity(name = "PersonAddress")
public static class PersonAddress implements Serializable {

	@Id
	@ManyToOne
	private Person person;

	@Id
	@ManyToOne
	private Address address;

	//Getters and setters are omitted for brevity

	@Override
	public boolean equals(Object o) {
		if ( this == o ) {
			return true;
		}
		if ( o == null || getClass() != o.getClass() ) {
			return false;
		}
		PersonAddress that = (PersonAddress) o;
		return Objects.equals( person, that.person ) &&
				Objects.equals( address, that.address );
	}

	@Override
	public int hashCode() {
		return Objects.hash( person, address );
	}
}

@Entity(name = "Address")
public static class Address implements Serializable {

	@Id
	@GeneratedValue
	private Long id;

	private String street;

	@Column(name = "`number`")
	private String number;

	private String postalCode;

	@OneToMany(
		mappedBy = "address",
		cascade = CascadeType.ALL,
		orphanRemoval = true
	)
	private List<PersonAddress> owners = new ArrayList<>();

	//Getters and setters are omitted for brevity

	@Override
	public boolean equals(Object o) {
		if ( this == o ) {
			return true;
		}
		if ( o == null || getClass() != o.getClass() ) {
			return false;
		}
		Address address = (Address) o;
		return Objects.equals( street, address.street ) &&
				Objects.equals( number, address.number ) &&
				Objects.equals( postalCode, address.postalCode );
	}

	@Override
	public int hashCode() {
		return Objects.hash( street, number, postalCode );
	}
}
```

这种方法在删除时只会触发一句sql：
```
person1.removeAddress( address1 );
```
```
DELETE FROM PersonAddress WHERE person_id = 1 AND address_id = 3
```

## 1.8. @NaturalId ##

### 1.8.1. 映射 ###
NaturalId 代表领域模型唯一标识符，比如身份证，电话号码，书籍的ISBN等等。一个实体类可以有多个
@NaturalId。

单个属性的 NaturalId：
```
@Entity(name = "Book")
public static class Book {

	@Id
	private Long id;

	private String title;

	private String author;

	@NaturalId
	private String isbn;

	//Getters and setters are omitted for brevity
}
```

带嵌入属性的 NaturalId：
```
@Entity(name = "Book")
public static class Book {

	@Id
	private Long id;

	private String title;

	private String author;

	@NaturalId
	@Embedded
	private Isbn isbn;

	//Getters and setters are omitted for brevity
}
@Embeddable
public static class Isbn implements Serializable {

	private String isbn10;

	private String isbn13;

	//Getters and setters are omitted for brevity

	@Override
	public boolean equals(Object o) {
		if ( this == o ) {
			return true;
		}
		if ( o == null || getClass() != o.getClass() ) {
			return false;
		}
		Isbn isbn = (Isbn) o;
		return Objects.equals( isbn10, isbn.isbn10 ) &&
				Objects.equals( isbn13, isbn.isbn13 );
	}

	@Override
	public int hashCode() {
		return Objects.hash( isbn10, isbn13 );
	}
}
```
多个属性的 NaturalId：
```
@Entity(name = "Book")
public static class Book {

	@Id
	private Long id;

	private String title;

	private String author;

	@NaturalId
	private String productNumber;

	@NaturalId
	@ManyToOne(fetch = FetchType.LAZY)
	private Publisher publisher;

	//Getters and setters are omitted for brevity
}

@Entity(name = "Publisher")
public static class Publisher implements Serializable {

	@Id
	private Long id;

	private String name;

	//Getters and setters are omitted for brevity

	@Override
	public boolean equals(Object o) {
		if ( this == o ) {
			return true;
		}
		if ( o == null || getClass() != o.getClass() ) {
			return false;
		}
		Publisher publisher = (Publisher) o;
		return Objects.equals( id, publisher.id ) &&
				Objects.equals( name, publisher.name );
	}

	@Override
	public int hashCode() {
		return Objects.hash( id, name );
	}
}
```

### 1.8.2. 使用 @NaturalId 加载 ###
```
Book book = entityManager
	.unwrap(Session.class)
	.byNaturalId( Book.class )
	.using( "isbn", "978-9730228236" )
	.load();

Book book = entityManager
	.unwrap(Session.class)
	.byNaturalId( Book.class )
	.using(
		"isbn",
		new Isbn(
			"973022823X",
			"978-9730228236"
		) )
	.load();

Book book = entityManager
	.unwrap(Session.class)
	.byNaturalId( Book.class )
	.using("productNumber", "973022823X")
	.using("publisher", publisher)
	.load();
```

因为前两个例子中都只有一个NaturalId，可以用简单的方式加载：
```
Book book = entityManager
	.unwrap(Session.class)
	.bySimpleNaturalId( Book.class )
	.load( "978-9730228236" );

Book book = entityManager
	.unwrap(Session.class)
	.bySimpleNaturalId( Book.class )
	.load(
		new Isbn(
			"973022823X",
			"978-9730228236"
		)
	);
```

NaturalIdLoadAccess提供2种不同的方法来获取实体：
- load() 获取对实体的引用，确保实体状态已初始化。
- getReference() 获取对该实体的引用。状态可以初始化也可以不初始化。如果该实体已经与当前正在运行的Session关联，则返回该引用（已加载或未加载）。如果该实体未在当前会话中加载并且该实体支持代理生成，则会生成并返回未初>始化的代理，否则从数据库加载该实体并返回。

### 1.8.3. 可变性和缓存 ###
默认情况下，NaturalId 是不可以更改的。如果要改变这一设定就要使用 `@NaturalId(mutable = true)`。在一个 session 中 Hibernate 维护着一个从 naturalId 到主键的映射，如果一个natural改变了，但是还没刷新上下文，这个映射关系可能会不准确：
```
Author author = entityManager
	.unwrap(Session.class)
	.bySimpleNaturalId( Author.class )
	.load( "john@acme.com" );

author.setEmail( "john.doe@acme.com" );

assertNull(
	entityManager
		.unwrap(Session.class)
		.bySimpleNaturalId( Author.class )
		.load( "john.doe@acme.com" )
);
```
为了在这种情况下能获取到正确的实体，应该加上这个方法 `setSynchronizationEnabled(true)`：
```
assertSame( author,
	entityManager
		.unwrap(Session.class)
		.bySimpleNaturalId( Author.class )
		.setSynchronizationEnabled( true )
		.load( "john.doe@acme.com" )
);
```

## 1.9. 继承 ##
尽管关系数据库系统不提供对继承的支持，但是Hibernate提供了几种策略来将这种面向对象的特征利用到域模型实体上：

- Mapped Super class
继承仅在域模型中实现，而没有在数据库模式中反映出来。只有子类有数据库表。

- Single table
域模型类层次结构被具体化为一个表，该表包含属于不同类类型的实体。

- Joined table
基类和所有子类都有自己的数据库表，并且获取子类实体也需要与父表进行联接。

- Table per calss
每个子类都有自己的表，其中包含子类和基类属性。

### 1.9.1. Mapped Super class ###
使用时MappedSuperclass，只有子类有数据库表，继承仅在域模型中可见，并且每个子类的数据库表都包含基类和子类属性。

```
@MappedSuperclass
public static class Account {
	@Id
	private Long id;
	private String owner;
	private BigDecimal balance;
	private BigDecimal interestRate;
	//Getters and setters are omitted for brevity
}

@Entity(name = "DebitAccount")
public static class DebitAccount extends Account {
	private BigDecimal overdraftFee;
	//Getters and setters are omitted for brevity
}

@Entity(name = "CreditAccount")
public static class CreditAccount extends Account {
	private BigDecimal creditLimit;
	//Getters and setters are omitted for brevity
}
```
数据库：
```
CREATE TABLE DebitAccount (
    id BIGINT NOT NULL ,
    balance NUMERIC(19, 2) ,
    interestRate NUMERIC(19, 2) ,
    owner VARCHAR(255) ,
    overdraftFee NUMERIC(19, 2) ,
    PRIMARY KEY ( id )
)

CREATE TABLE CreditAccount (
    id BIGINT NOT NULL ,
    balance NUMERIC(19, 2) ,
    interestRate NUMERIC(19, 2) ,
    owner VARCHAR(255) ,
    creditLimit NUMERIC(19, 2) ,
    PRIMARY KEY ( id )
)
```

### 1.9.2. Single table ###
单表继承策略将所有子类仅映射到一个数据库表。每个子类声明其自己的持久属性。当省略显式继承策略（例如@Inheritance）时，JPA 将选择 SINGLE_TABLE 作默认策略。
```
@Entity(name = "Account")
@Inheritance(strategy = InheritanceType.SINGLE_TABLE)
public static class Account {
	@Id
	private Long id;
	private String owner;
	private BigDecimal balance;
	private BigDecimal interestRate;
	//Getters and setters are omitted for brevity
}

@Entity(name = "DebitAccount")
public static class DebitAccount extends Account {
	private BigDecimal overdraftFee;
	//Getters and setters are omitted for brevity
}

@Entity(name = "CreditAccount")
public static class CreditAccount extends Account {
	private BigDecimal creditLimit;
	//Getters and setters are omitted for brevity
}
```
数据库：
```
CREATE TABLE Account (
    DTYPE VARCHAR(31) NOT NULL ,
    id BIGINT NOT NULL ,
    balance NUMERIC(19, 2) ,
    interestRate NUMERIC(19, 2) ,
    owner VARCHAR(255) ,
    overdraftFee NUMERIC(19, 2) ,
    creditLimit NUMERIC(19, 2) ,
    PRIMARY KEY ( id )
)
```

### 1.9.3. Joined Table ###
超类和子类有各自的表。每个子类都拥有一个指向超类的外键，并且这个外键是子类的主键。可以用 @PrimaryKeyJoinColumn 来指定外键列的名称。

```
@Entity(name = "Account")
@Inheritance(strategy = InheritanceType.JOINED)
public static class Account {
	@Id
	private Long id;
	private String owner;
	private BigDecimal balance;
	private BigDecimal interestRate;
	//Getters and setters are omitted for brevity
}

@Entity(name = "DebitAccount")
public static class DebitAccount extends Account {
	private BigDecimal overdraftFee;
	//Getters and setters are omitted for brevity
}

@Entity(name = "CreditAccount")
public static class CreditAccount extends Account {
	private BigDecimal creditLimit;
	//Getters and setters are omitted for brevity
}
```
数据库：
```
CREATE TABLE Account (
    id BIGINT NOT NULL ,
    balance NUMERIC(19, 2) ,
    interestRate NUMERIC(19, 2) ,
    owner VARCHAR(255) ,
    PRIMARY KEY ( id )
)

CREATE TABLE CreditAccount (
    creditLimit NUMERIC(19, 2) ,
    id BIGINT NOT NULL ,
    PRIMARY KEY ( id )
)

CREATE TABLE DebitAccount (
    overdraftFee NUMERIC(19, 2) ,
    id BIGINT NOT NULL ,
    PRIMARY KEY ( id )
)

ALTER TABLE CreditAccount
ADD CONSTRAINT FKihw8h3j1k0w31cnyu7jcl7n7n
FOREIGN KEY (id) REFERENCES Account

ALTER TABLE DebitAccount
ADD CONSTRAINT FKia914478noepymc468kiaivqm
FOREIGN KEY (id) REFERENCES Account
```

### 1.9.4. Table Per Class ###
子类和超类都会生成表。子表显示拥有超类的属性。

略。

# 2. 引导程序 #

## 2.1. 构建 ServiceRegistry ##
有两种 ServiceRegistry, BootstrapServiceRegistry 和 StandardServiceRegistry。他们主要管理Hibernate 在引导和运行时所需要的依赖服务：
- ClassLoaderService，它控制 Hibernate 与 ClassLoader 的交互方式。
- IntegratorService，控制 Integrator 实例的管理和发现。
- StrategySelector，控制了Hibernate 如何解决各种 strategy 的实现。

一般不需要定制化这些 service，直接使用 Hibernate 默认配置。
```
BootstrapServiceRegistry bootstrapRegistry = new BootstrapServiceRegistryBuilder().build();
```
```
ServiceRegistry standardRegistry = new StandardServiceRegistryBuilder().build();
```

## 2.2. 监听 ##
略。

## 2.3. 配置元数据 ##
本机自举的第二步是构建一个org.hibernate.boot.Metadata对象，用来注册实体模型和映射关系。
```
ServiceRegistry standardRegistry =
        new StandardServiceRegistryBuilder().build();

MetadataSources sources = new MetadataSources( standardRegistry )
    .addAnnotatedClass( MyEntity.class )
    .addAnnotatedClassName( "org.hibernate.example.Customer" )
    .addResource( "org/hibernate/example/Order.hbm.xml" )
    .addResource( "org/hibernate/example/Product.orm.xml" );
```
如果需要更多的控制映射过程，使用 getMetadataBuilder()：
```
Metadata metadata = new MetadataSources( standardRegistry )
    .addAnnotatedClass( MyEntity.class )
    .addAnnotatedClassName( "org.hibernate.example.Customer" )
    .addResource( "org/hibernate/example/Order.hbm.xml" )
    .addResource( "org/hibernate/example/Product.orm.xml" )
    .getMetadataBuilder()
    .applyImplicitNamingStrategy( ImplicitNamingStrategyJpaCompliantImpl.INSTANCE )
    .build();
```

## 2.4. SessionFactroy ##
第三步构建 SessionFactory：
```
StandardServiceRegistry standardRegistry = new StandardServiceRegistryBuilder()
    .configure( "org/hibernate/example/hibernate.cfg.xml" )
    .build();

Metadata metadata = new MetadataSources( standardRegistry )
    .addAnnotatedClass( MyEntity.class )
    .addAnnotatedClassName( "org.hibernate.example.Customer" )
    .addResource( "org/hibernate/example/Order.hbm.xml" )
    .addResource( "org/hibernate/example/Product.orm.xml" )
    .getMetadataBuilder()
    .applyImplicitNamingStrategy( ImplicitNamingStrategyJpaCompliantImpl.INSTANCE )
    .build();

SessionFactory sessionFactory = metadata.getSessionFactoryBuilder()
    .applyBeanManager( getBeanManager() )
    .build();
```

# 3. Schema Generation #

Hibernate允许您从实体映射生成数据库。传统上，从实体映射生成架构的过程称为HBM2DDL。
对于下列的实体：
```
@Entity(name = "Customer")
public class Customer {
	@Id
	private Integer id;
	private String name;
	@Basic( fetch = FetchType.LAZY )
	private UUID accountsPayableXrefId;
	@Lob
	@Basic( fetch = FetchType.LAZY )
	@LazyGroup( "lobs" )
	private Blob image;
	//Getters and setters are omitted for brevity
}

@Entity(name = "Person")
public static class Person {
	@Id
	private Long id;
	private String name;
	@OneToMany(mappedBy = "author")
	private List<Book> books = new ArrayList<>();
	//Getters and setters are omitted for brevity
}

@Entity(name = "Book")
public static class Book {
	@Id
	private Long id;
	private String title;
	@NaturalId
	private String isbn;
	@ManyToOne
	private Person author;
	//Getters and setters are omitted for brevity
}
```
如果hibernate.hbm2ddl.auto配置设置为create，则Hibernate将生成以下数据库架构：
```
create table Customer (
    id integer not null,
    accountsPayableXrefId binary,
    image blob,
    name varchar(255),
    primary key (id)
)

create table Book (
    id bigint not null,
    isbn varchar(255),
    title varchar(255),
    author_id bigint,
    primary key (id)
)

create table Person (
    id bigint not null,
    name varchar(255),
    primary key (id)
)

alter table Book
    add constraint UK_u31e1frmjp9mxf8k8tmp990i unique (isbn)

alter table Book
    add constraint FKrxrgiajod1le3gii8whx2doie
    foreign key (author_id)
    references Person
```

## 3.1. 导入脚本文件 ##

例如，考虑以下schema-generation.sql导入文件：
```
create sequence book_sequence start with 1 increment by 1
```
如果我们将Hibernate配置为导入上面的脚本：
```
<property name="hibernate.hbm2ddl.import_files" value="schema-generation.sql" />
```
模式自动生成后，Hibernate将执行脚本文件。

## 3.2. 数据库层的检查 ##

Hibernate提供了@Check注释，以便您可以指定一个任意的 SQL CHECK 约束，该约束可以如下定义：
```
@Entity(name = "Book")
@Check( constraints = "CASE WHEN isbn IS NOT NULL THEN LENGTH(isbn) = 13 ELSE true END")
public static class Book {
	@Id
	private Long id;
	private String title;
	@NaturalId
	private String isbn;
	private Double price;
	//Getters and setters omitted for brevity
}
```
如果现在持久化一个 Book 实例，而 isbn 没有达到 13 位，就会抛错。


## 3.3. 数据库列的默认值 ##
使用Hibernate，您可以使用@ColumnDefault注释为给定的数据库列指定默认值。

@DynamicInsert属性：设置为 true，表示 insert 对象的时候，生成动态的 insert 语句，如果这个字段的值是 null 就不会加入到 insert 语句当中.默认 false。数据库插入日期或时间戳字段时，在对象字段为空的情况下，表字段能自动填写当前的 sysdate。

@DynamicUpdate属性：设置为 true，表示 update 对象的时候，生成动态的update语句，如果这个字段的值是 null 就不会被加入到 update 语句中，默认 false。

```
@Entity(name = "Person")
@DynamicInsert
public static class Person {

    @Id
    private Long id;

    @ColumnDefault("'N/A'")
    private String name;

    @ColumnDefault("-1")
    private Long clientId;

    //Getter and setters omitted for brevity

}
```

## 3.4. 唯一约束 ##
@UniqueConstraint：
```
@Entity
@Table(
    name = "book",
    uniqueConstraints =  @UniqueConstraint(
        name = "uk_book_title_author",
        columnNames = {
            "title",
            "author_id"
        }
    )
)
public static class Book {

    @Id
    @GeneratedValue
    private Long id;

    private String title;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(
        name = "author_id",
        foreignKey = @ForeignKey(name = "fk_book_author_id")
    )
    private Author author;

    //Getter and setters omitted for brevity
}

@Entity
@Table(name = "author")
public static class Author {

    @Id
    @GeneratedValue
    private Long id;

    @Column(name = "first_name")
    private String firstName;

    @Column(name = "last_name")
    private String lastName;

    //Getter and setters omitted for brevity
}
```

## 3.5. 索引 ##
@Index注释用于创建数据库索引：
```
@Entity
@Table(
    name = "author",
    indexes =  @Index(
        name = "idx_author_first_last_name",
        columnList = "first_name, last_name",
        unique = false
    )
)
public static class Author {
    @Id
    @GeneratedValue
    private Long id;
    @Column(name = "first_name")
    private String firstName;
    @Column(name = "last_name")
    private String lastName;
    //Getter and setters omitted for brevity
}
```
```
create table author (
    id bigint not null,
    first_name varchar(255),
    last_name varchar(255),
    primary key (id)
)

create index idx_author_first_last_name on author (first_name, last_name)
```

# 4. 持久性上下文 #
org.hibernate.SessionAPI 及 javax.persistence.EntityManagerAPI 代表了处理持久性数据的上下文。持久性数据具有与持久性上下文和基础数据库相关联的状态：

- transient
该实体刚刚被实例化，并且与持久性上下文无关。它在数据库中没有持久性表示形式，并且通常没有分配标识符值（除非使用了分配的生成器）。
- managed or persistent
该实体具有关联的标识符，并且与持久性上下文关联。它在数据库中可能存在也可能不存在。
- detached
实体具有关联的标识符，但不再与持久性上下文关联（通常是因为持久性上下文已关闭或实例已从上下文中退出）
- removed
该实体具有关联的标识符，并且与持久性上下文关联，但是已安排将其从数据库中删除

## 4.1. 使实体持久化 ##
entityManager.persist

session.save

## 4.2. 删除实体 ##
entityManager.remove

session.delete
## 4.3. 获取实体 ##
懒加载：
entityManager.getReference(?.class)
session.load(?.class)

非懒加载：
entityManager.find(?.class)
session.get(?.class)
session.byId(?.class).load(?)
返回Optional：session.byId(?.class).loadOptional(?)

通过naturalId:
session.bySimpleNaturalId(?.class).
session.byNaturalId(?.class).using("?", ?).load()
返回Optional：session.byNaturalId(?.class).using("?", ?).loadOptional()

获取多个实体：
session.byMultipleIds( Person.class ).multiLoad( 1L, 2L, 3L );

## 4.4. 过滤实体 ##

- 静态（例如@Where和@WhereJoinTable）
它们是在映射时定义的，无法在运行时更改。

- 动态（例如@Filter和@FilterJoinTable）
在运行时应用和配置。
