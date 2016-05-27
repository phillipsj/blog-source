---
title: DapperExtensions Mapper to Dapper
tags:
  - .NET
  - Dapper
  - ORM
date: 2015-02-18 19:00:00
---

# DapperExtensions Mapper to Dapper

I have used several of the differnet Object Relation Mappers (ORMs) out there
and I find myself going back to [Dapper](https://github.com/StackExchange/dapper-dot-net). Dapper provides all the functionality
that I find that I need and gives the the full power of the best DSL for working
with a database, SQL. It is fast and lightweight. However, I do not know many
peeps that want to waste there time writing the typical CRUD operations. Projects
like [DapperExtensions](https://github.com/tmsmith/Dapper-Extensions), Dapper.Contrib, Dapper.Rainbow, and others, give users
plenty of choices.

I like using DapperExtensions because I like class mappings to map my POCOs
to tables since most legacy databases typically do not follow C# naming
conventions. The other libraries use attributes, which is fine, but not an
approach I typically like. The big issue that arises is DapperExtensions use
the ClassMapper to generate SQL statements aliasing the columns. This works until
you need to fallback to just plain Dapper. So to solve this issue and still use
the mapping files, I created a custom ClassMapper and implemented the ITypeMap
interface so that way I could use my mapping file to set the type map on the
Dapper SqlMapper.

First step is to create a class that implements IMemberMap from Dapper.

> <div><div class="highlight-csharp"><div class="highlight"><pre><span class="k">public</span> <span class="k">class</span> <span class="nc">CustomMemberMap</span> <span class="p">:</span> <span class="n">SqlMapper</span><span class="p">.</span><span class="n">IMemberMap</span>
> <span class="p">{</span>
>     <span class="k">private</span> <span class="k">readonly</span> <span class="n">MemberInfo</span> <span class="n">_member</span><span class="p">;</span>
>     <span class="k">private</span> <span class="k">readonly</span> <span class="kt">string</span> <span class="n">_columnName</span><span class="p">;</span>
> 
>     <span class="k">public</span> <span class="nf">CustomMemberMap</span><span class="p">(</span><span class="n">MemberInfo</span> <span class="n">member</span><span class="p">,</span> <span class="kt">string</span> <span class="n">columnName</span><span class="p">)</span>
>     <span class="p">{</span>
>         <span class="k">this</span><span class="p">.</span><span class="n">_member</span> <span class="p">=</span> <span class="n">member</span><span class="p">;</span>
>         <span class="k">this</span><span class="p">.</span><span class="n">_columnName</span> <span class="p">=</span> <span class="n">columnName</span><span class="p">;</span>
>     <span class="p">}</span>
> 
>     <span class="k">public</span> <span class="kt">string</span> <span class="n">ColumnName</span>
>     <span class="p">{</span>
>         <span class="k">get</span> <span class="p">{</span> <span class="k">return</span> <span class="n">_columnName</span><span class="p">;</span> <span class="p">}</span>
>     <span class="p">}</span>
> 
>     <span class="k">public</span> <span class="n">FieldInfo</span> <span class="n">Field</span>
>     <span class="p">{</span>
>         <span class="k">get</span> <span class="p">{</span> <span class="k">return</span> <span class="n">_member</span> <span class="k">as</span> <span class="n">FieldInfo</span><span class="p">;</span> <span class="p">}</span>
>     <span class="p">}</span>
> 
>     <span class="k">public</span> <span class="n">Type</span> <span class="n">MemberType</span>
>     <span class="p">{</span>
>         <span class="k">get</span>
>         <span class="p">{</span>
>             <span class="k">switch</span> <span class="p">(</span><span class="n">_member</span><span class="p">.</span><span class="n">MemberType</span><span class="p">)</span>
>             <span class="p">{</span>
>                 <span class="k">case</span> <span class="n">MemberTypes</span><span class="p">.</span><span class="n">Field</span><span class="p">:</span>
>                     <span class="k">return</span> <span class="p">((</span><span class="n">FieldInfo</span><span class="p">)</span><span class="n">_member</span><span class="p">).</span><span class="n">FieldType</span><span class="p">;</span>
>                 <span class="k">case</span> <span class="n">MemberTypes</span><span class="p">.</span><span class="n">Property</span><span class="p">:</span>
>                     <span class="k">return</span> <span class="p">((</span><span class="n">PropertyInfo</span><span class="p">)</span><span class="n">_member</span><span class="p">).</span><span class="n">PropertyType</span><span class="p">;</span>
>                 <span class="k">default</span><span class="p">:</span>
>                     <span class="k">throw</span> <span class="k">new</span> <span class="nf">NotSupportedException</span><span class="p">();</span>
>             <span class="p">}</span>
>         <span class="p">}</span>
>     <span class="p">}</span>
> 
>     <span class="k">public</span> <span class="n">ParameterInfo</span> <span class="n">Parameter</span>
>     <span class="p">{</span>
>         <span class="k">get</span> <span class="p">{</span> <span class="k">return</span> <span class="k">null</span><span class="p">;</span> <span class="p">}</span>
>     <span class="p">}</span>
> 
>     <span class="k">public</span> <span class="n">PropertyInfo</span> <span class="n">Property</span>
>     <span class="p">{</span>
>         <span class="k">get</span> <span class="p">{</span> <span class="k">return</span> <span class="n">_member</span> <span class="k">as</span> <span class="n">PropertyInfo</span><span class="p">;</span> <span class="p">}</span>
>     <span class="p">}</span>
> <span class="p">}</span>
> </pre></div>
> </div>
> </div>

Now that you have a member map in place, now it is type to create your custom
DapperExtensions ClassMapper.

> <div><div class="highlight-csharp"><div class="highlight"><pre><span class="k">public</span> <span class="k">class</span> <span class="nc">CustomClassMapper</span><span class="p">&lt;</span><span class="n">T</span><span class="p">&gt;:</span> <span class="n">ClassMapper</span><span class="p">&lt;</span><span class="n">T</span><span class="p">&gt;,</span>
>                                     <span class="n">SqlMapper</span><span class="p">.</span><span class="n">ITypeMap</span> <span class="k">where</span> <span class="n">T</span> <span class="p">:</span> <span class="k">class</span>
> <span class="p">{</span>
>     <span class="k">public</span> <span class="n">ConstructorInfo</span> <span class="nf">FindConstructor</span><span class="p">(</span><span class="kt">string</span><span class="p">[]</span> <span class="n">names</span><span class="p">,</span> <span class="n">Type</span><span class="p">[]</span> <span class="n">types</span><span class="p">)</span>
>     <span class="p">{</span>
>         <span class="k">return</span> <span class="k">this</span><span class="p">.</span><span class="n">EntityType</span><span class="p">.</span><span class="n">GetConstructor</span><span class="p">(</span><span class="n">Type</span><span class="p">.</span><span class="n">EmptyTypes</span><span class="p">);</span>
>     <span class="p">}</span>
> 
>     <span class="k">public</span> <span class="n">ConstructorInfo</span> <span class="nf">FindExplicitConstructor</span><span class="p">()</span>
>     <span class="p">{</span>
>        <span class="k">return</span> <span class="k">null</span><span class="p">;</span>
>     <span class="p">}</span>
> 
>     <span class="k">public</span> <span class="n">SqlMapper</span><span class="p">.</span><span class="n">IMemberMap</span> <span class="n">GetConstructorParameter</span><span class="p">(</span>
>                         <span class="n">ConstructorInfo</span> <span class="n">constructor</span><span class="p">,</span> <span class="kt">string</span> <span class="n">columnName</span><span class="p">)</span>
>     <span class="p">{</span>
>         <span class="k">return</span> <span class="k">null</span><span class="p">;</span>
>     <span class="p">}</span>
> 
>     <span class="k">public</span> <span class="n">SqlMapper</span><span class="p">.</span><span class="n">IMemberMap</span> <span class="n">GetMember</span><span class="p">(</span><span class="kt">string</span> <span class="n">columnName</span><span class="p">)</span>
>     <span class="p">{</span>
>         <span class="k">if</span> <span class="p">(</span><span class="n">Properties</span><span class="p">.</span><span class="n">SingleOrDefault</span><span class="p">(</span><span class="n">x</span> <span class="p">=&gt;</span>
>                         <span class="n">x</span><span class="p">.</span><span class="n">ColumnName</span> <span class="p">==</span> <span class="n">columnName</span><span class="p">)</span> <span class="p">!=</span> <span class="k">null</span><span class="p">)</span>
>         <span class="p">{</span>
>             <span class="k">return</span> <span class="k">new</span> <span class="nf">CustomMemberMap</span><span class="p">(</span>
>                         <span class="k">this</span><span class="p">.</span><span class="n">EntityType</span><span class="p">.</span><span class="n">GetMember</span><span class="p">(</span><span class="n">Properties</span><span class="p">.</span><span class="n">First</span><span class="p">(</span><span class="n">x</span> <span class="p">=&gt;</span>
>                             <span class="n">x</span><span class="p">.</span><span class="n">ColumnName</span> <span class="p">==</span> <span class="n">columnName</span><span class="p">).</span><span class="n">Name</span><span class="p">).</span><span class="n">Single</span><span class="p">(),</span>
>                         <span class="n">columnName</span><span class="p">);</span>
>         <span class="p">}</span>
>         <span class="k">else</span>
>         <span class="p">{</span>
>             <span class="k">return</span> <span class="k">null</span><span class="p">;</span>
>         <span class="p">}</span>
>     <span class="p">}</span>
> <span class="p">}</span>
> </pre></div>
> </div>
> </div>

Now that you have both a CustomClassMapper that also implements ITypeMap from
Dapper you can now input the following code write before you execute a Dapper
method when you need to fall back from DapperExtensions to Dapper.

> <div><div class="highlight-csharp"><div class="highlight"><pre><span class="c1">// Note that Person is a POCO and PersonMap is your mapping file</span>
> <span class="n">SqlMapper</span><span class="p">.</span><span class="n">SetTypeMap</span><span class="p">(</span><span class="k">typeof</span><span class="p">(</span><span class="n">Person</span><span class="p">),</span> <span class="k">new</span> <span class="n">PersonMap</span><span class="p">());</span>
> </pre></div>
> </div>
> </div>

I hope others find this useful when needing to use Dapper with DapperExtensions.