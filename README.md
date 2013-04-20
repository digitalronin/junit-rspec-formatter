## junit-rspec-formatter

An output formatter for running JUnit tests which outputs '.' for each passing test, and 'F' for each failure

## The Problem

TDD is great, but when I'm writing Java code, I don't like the default output format from running JUnit tests;

    [junit] Running com.whatever.MyGreatTest
    [junit] Tests run: 11, Failures: 0, Errors: 0, Time elapsed: 0.062 sec
    [junit] Running com.whatever.AnotherTest
    [junit] Tests run: 8, Failures: 0, Errors: 0, Time elapsed: 0.074 sec
    ...

When you have a lot of test suites to run, this output makes it harder to spot failures and identify exactly what has broken.
Instead, I want something more like the output I get from Rspec when working in Ruby;

    $ rspec spec/my_great_spec.rb
    ..............................

    Finished in 3.95 seconds
    30 examples, 0 failures


Each dot is a passing test. If a single test fails, the output would look something like this;

    $ rspec spec/my_great_spec.rb
    ........F.....................

    (Followed by details about exactly what the failure was)


I find it a lot easier to read this - it makes test failures must quicker to identify.

So, I want a JUnit output formatter that produces Rspec-like output when running tests. This is my attempt to write one.

## Output

This is as close as I've been able to get, so far;


    [junit] .
    [junit]   Tests run: 1, Failures: 0, Errors: 0, Time elapsed: 0.668 sec
    [junit]
    [junit] ...........
    [junit]   Tests run: 11, Failures: 0, Errors: 0, Time elapsed: 0.065 sec
    [junit]
    [junit] ........
    [junit]   Tests run: 8, Failures: 0, Errors: 0, Time elapsed: 0.889 sec
    [junit]

It's not ideal, but it's good enough for me.


## Installation

Put the jar file into your project somewhere where it will be compiled and available to your tests (e.g. test/lib/).


## Usage

In your build.xml file, have something like this;


    <target name="spec" depends="[whatever dependencies your tests have]">
      <junit printsummary="off" fork="on"
        failureproperty="test.failed" showoutput="off" dir="out"
        outputtoformatters="false" filtertrace="on" >

        <classpath refid="test.classpath" />
        <formatter classname="com.digitalronin.RspecFormatter" usefile="false" />
        <batchtest fork="on" filtertrace="on">
          <fileset dir="${test-classes.dir}">
            <include name="**/*Test*" />
          </fileset>
        </batchtest>
      </junit>
      <fail if="test.failed">tests.failed=${test.failed}</fail>
    </target>


## Bugs/TODO

There are lots of ways this could be improved. See the comments at the top of the source.


## LICENSE

The MIT License (MIT)

Copyright (C) 2013 David Salgado

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
