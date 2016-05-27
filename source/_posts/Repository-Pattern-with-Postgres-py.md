---
title: Repository Pattern with Postgres.py
tags:
  - Python
  - Postgres
date: 2014-03-04 19:00:00
---

# Repository Pattern with Postgres.py

At [PyTennessee](http://www.pytennessee.org/) I attended a presentation about ‘Postgres.py’_.  It is a nice simple
interface.  The presentation and one of the project creators was no other
than ‘Chad Whitacre’_.  If you haven’t met Chad Whitacre then you are
missing out.  He is a great example of someone who is truly an
ambassador of the community.  During the presentation
there were lots of discussion about accessing data and it seems there
was a focus on using the ‘active record’_ pattern.  However, I feel that
the library would lend itself to the use of the [repository](http://martinfowler.com/eaaCatalog/repository.html) pattern.
Without any more here is the [gist](https://gist.github.com/phillipsj/9367366).

> <div><div class="highlight-python"><div class="highlight"><pre><span class="kn">from</span> <span class="nn">postgres.orm</span> <span class="kn">import</span> <span class="n">Model</span>
> <span class="kn">from</span> <span class="nn">postgres</span> <span class="kn">import</span> <span class="n">Postgres</span>
> 
> <span class="k">class</span> <span class="nc">Foo</span><span class="p">(</span><span class="n">Model</span><span class="p">):</span>
>         <span class="n">typname</span> <span class="o">=</span> <span class="s">"foo"</span>
> 
> <span class="k">class</span> <span class="nc">FooRepository</span><span class="p">(</span><span class="n">connection_string</span><span class="p">):</span>
>         <span class="k">def</span> <span class="nf">__init__</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
>                 <span class="bp">self</span><span class="o">.</span><span class="n">db</span> <span class="o">=</span> <span class="n">Postgres</span><span class="p">(</span><span class="n">connection_string</span><span class="p">)</span>
>                 <span class="bp">self</span><span class="o">.</span><span class="n">db</span><span class="o">.</span><span class="n">register_model</span><span class="p">(</span><span class="n">Foo</span><span class="p">)</span>
> 
>         <span class="k">def</span> <span class="nf">get_all</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
>                 <span class="k">return</span> <span class="bp">self</span><span class="o">.</span><span class="n">db</span><span class="o">.</span><span class="n">all</span><span class="p">(</span><span class="s">"SELECT foo.*::foo FROM foo"</span><span class="p">)</span>
> 
> <span class="n">repo</span> <span class="o">=</span> <span class="n">FooRepository</span><span class="p">(</span><span class="s">"&lt;connection-string&gt;"</span><span class="p">)</span>
> 
> <span class="n">foos</span> <span class="o">=</span> <span class="n">repo</span><span class="o">.</span><span class="n">get_all</span><span class="p">()</span>
> </pre></div>
> </div>
> </div>