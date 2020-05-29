+++
date = "2020-05-16 09:51:00+00:00"
draft = false
tags = ["python", "testing", "pytest"]
title = "Parameterize Python Tests"
url = "/post/618264170735009792/parameterize-python-tests"
+++
### Introduction

A single test case follows a pattern. Setup the data, invoke the function or method with the arguments and assert the return data or the state changes. A function will have a minimum of one or more test cases for various success and failure cases.

Here is an example implementation of `` wc `` command for a single file that returns `` number of words, lines, and characters `` for an ASCII text file.

    def wc_single(path, words=False, chars=False, lines=False):
        """Implement GNU/Linux `wc` command
           behavior for a single file.
        """
        res = {}
        try:
            with open(path, 'r') as fp:
                # Consider, the file is a small file.
                content = fp.read()

                if words:
                    lines = content.split('\n')
                    res['words'] = sum([1
                                        for line in lines
                                        for word in line.split(' ')
                                        if len(word.strip()) >= 1])

                if chars:
                    res['chars'] = len(content)

                if lines:
                    res['lines'] = len(content.strip().split('\n'))

                return res
        except FileNotFoundError as exc:
            print(f'No such file {path}')
            return res

For the scope of the blog post, consider the implementation as sufficient and not complete. I’m aware; for example, the code would return the number of lines as 1 for the empty file. For simplicity, consider the following six test cases for `` wc_single `` function.

1.   A test case with the file missing.
2.   A test case with a file containing some data, with `` words, chars, lines `` set to `` True. ``
3.   A test case with a file containing some data, with `` words `` alone set to `` True. ``
4.   A test case with a file containing some data, with `` lines `` alone set to `` True. ``
5.   A test case with a file containing some data, with `` chars `` alone set to `` True. ``
6.   A test case with a file containing some data, with `` words, chars, lines `` alone set to `` True. ``

I’m skipping the combinatorics values for two of the three arguments to be `` True `` for simplicity.

### Test Code

The `` file.txt `` content (don’t worry about the comma after `` knows ``)

    Welcome to the new normal.
    No one knows, what is new normal.


    Hang on!

`` wc `` output for `` file.txt ``

    $wc file.txt
    5      14      72 file.txt

Here is the implementation of the six test cases.

    class TestWCSingleWithoutParameterized(unittest.TestCase):
        def test_with_missing_file(self):
            with patch("sys.stdout", new=StringIO()) as output:
                path = 'missing.txt'
                res = wc_single(path)

                assert f'No such file {path}' in output.getvalue().strip()

        def test_for_all_three(self):
            res = wc_single('file.txt', words=True, chars=True, lines=True)

            assert res == {'words': 14, 'lines': 5, 'chars': 72}

        def test_for_words(self):
            res = wc_single('file.txt', words=True)

            assert res == {'words': 14}

        def test_for_chars(self):
            res = wc_single('file.txt', chars=True)

            assert res == {'chars': 72}

        def test_for_lines(self):
            res = wc_single('file.txt', lines=True)

            assert res == {'lines': 5}

        def test_default(self):
            res = wc_single('file.txt')

            assert res == {}


The test case follows the pattern, setup the data, invoke the function with arguments, and asserts the return or printed value. Most of the testing code is a copy-paste code from the previous test case.

### Parameterize

It’s possible to reduce all six methods into a single test method with <a href="https://github.com/wolever/parameterized" target="_blank">parameterized libray</a>. Rather than having six methods, a decorator can inject the _data for tests, expected value_ after the test. Here is how the code after parameterization.

    def get_params():
        """Return a list of parameters for each test case."""
        missing_file = 'missing.txt'
        test_file = 'file.txt'
        return [('missing_file',
                {'path': missing_file},
                f'No such file {missing_file}',
                True),

                ('all_three',
                {'path': test_file, 'words': True, 'lines': True, 'chars': True},
                 {'words': 14, 'lines': 5, 'chars': 72},
                 False),

                ('only_words',
                {'path': test_file, 'words': True},
                {'words': 14},
                False),

                ('only_chars',
                {'path': test_file, 'chars': True},
                {'chars': 72}, False),

                ('only_lines',
                {'path': test_file, 'lines': True},
                {'lines': 5},
                False),

                ('default',
                {'path': test_file},
                {},
                False)
        ]

    class TestWCSingleWithParameterized(unittest.TestCase):
        @parameterized.expand(get_params())
        def test_wc_single(self, _, kwargs, expected, stdout):
            with patch("sys.stdout", new=StringIO()) as output:
                res = wc_single(**kwargs)

                if stdout:
                    assert expected in output.getvalue().strip()
                else:
                    assert expected == res


The pytest runner output

     pytest -s -v wc.py::TestWCSingleWithParameterized
    ============================================================================================================ test session starts =============================================================================================================
    platform darwin -- Python 3.6.9, pytest-5.4.1, py-1.8.1, pluggy-0.13.1 -- /usr/local/Caskroom/miniconda/base/envs/hotbox/bin/python
    cachedir: .pytest_cache
    rootdir: /Users/user/code/snippets
    plugins: kubetest-0.6.4, cov-2.8.1
    plugins: kubetest-0.6.4, cov-2.8.1
    collected 6 items

    wc.py::TestWCSingleWithParameterized::test_wc_single_0_missing_file PASSED
    wc.py::TestWCSingleWithParameterized::test_wc_single_1_all_three PASSED
    wc.py::TestWCSingleWithParameterized::test_wc_single_2_only_words PASSED
    wc.py::TestWCSingleWithParameterized::test_wc_single_3_only_chars PASSED
    wc.py::TestWCSingleWithParameterized::test_wc_single_4_only_lines PASSED
    wc.py::TestWCSingleWithParameterized::test_wc_single_5_default PASSED

### What did `` @parameterized.expand `` do?

The decorator collected all the arguments to pass the test method, `` test_wc_single. `` When pytest runner ran the test class, the decorator injected a new function name following default rule, `` <func_name>_<param_number>_<first_argument_to_pass> ``. Then each item in the list returned by `` get_params `` to the test case, `` test_wc_single. ``

### What did `` get_params `` return?

`` get_params `` returns a list(iterable). Each item in the list is a bunch of parameters for _each test case_.

`` ('missing_file', {'path': missing_file}, f'No such file {missing_file}', True) ``

Each item in the list is a tuple containing the parameters for a test case. Let’s take the first tuple as an example.

First item in the tuple is a function suffix used while printing the function name(`` 'missing_file' ``). The second value in the tuple is a dictionary containing the arguments to call the _wc\_single_ function(`` {'path': missing_file} ``). Each test case passes a different number of arguments to the `` wc_single. `` Hence the second item in the first and second tuple has different keys in the dictionary. The third item in the tuple is the _expected_ value to assert after calling the testing function(`` f'No such file {missing_file}' ``). The fourth item in the tuple determines what to assert after the function call, the return value, or stdout(`` True ``).

The principle is decorator will expand and pass each item in the iterable to the test method. Then how to receive the parameter and structure the code is up to the programmer.

### Can I change the function name printed?

Yes, the `` parameterized.expand `` accepts a function as a value to the argument `` name_func ``, which can change each function name.

Here is the custom name function which suffixes the first argument for each test in the function name

    def custom_name_func(testcase_func, param_num, param):
        return f"{testcase_func.__name__}_{parameterized.to_safe_name(param.args[0])}"

    class TestWCSingleWithParameterized(unittest.TestCase):
        @parameterized.expand(get_params(),
                              name_func=custom_name_func)
        def test_wc_single(self, _, kwargs, expected, stdout):
            with patch("sys.stdout", new=StringIO()) as output:
                res = wc_single(**kwargs)

                if stdout:
                    assert expected in output.getvalue().strip()
                else:
                    assert expected == res

Test runner output

    $pytest -s -v wc.py::TestWCSingleWithParameterized
    ...
    wc.py::TestWCSingleWithParameterized::test_wc_single_all_three PASSED
    wc.py::TestWCSingleWithParameterized::test_wc_single_default PASSED
    wc.py::TestWCSingleWithParameterized::test_wc_single_missing_file PASSED
    wc.py::TestWCSingleWithParameterized::test_wc_single_only_chars PASSED
    wc.py::TestWCSingleWithParameterized::test_wc_single_only_lines PASSED
    wc.py::TestWCSingleWithParameterized::test_wc_single_only_words PASSED

### Is it possible to run a single test after parameterization?

Yes. You should give the full generated name and rather than _actual method name in the code_.

    $pytest -s -v "wc.py::TestWCSingleWithParameterized::test_wc_single_all_three"
    ...
    wc.py::TestWCSingleWithParameterized::test_wc_single_all_three PASSED

Not like these

    $pytest -s -v wc.py::TestWCSingleWithParameterized::test_wc_single
    ...
    =========================================================================================================== no tests ran in 0.24s ============================================================================================================
    ERROR: not found: /Users/user/code/snippets/wc.py::TestWCSingleWithParameterized::test_wc_single
    (no name '/Users/user/code/snippets/wc.py::TestWCSingleWithParameterized::test_wc_single' in any of [<UnitTestCase TestWCSingleWithParameterized>])
    $pytest -s -v "wc.py::TestWCSingleWithParameterized::test_wc_single_*"
    ...
    =========================================================================================================== no tests ran in 0.22s ============================================================================================================
    ERROR: not found: /Users/user/code/snippets/wc.py::TestWCSingleWithParameterized::test_wc_single_*
    (no name '/Users/user/code/snippets/wc.py::TestWCSingleWithParameterized::test_wc_single_*' in any of [<UnitTestCase TestWCSingleWithParameterized>])


### Does test failure give enough information to debug?

Yes, Here is an example of a test failure.

    $pytest -s -v wc.py
    ...
    _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

    self = <wc.TestWCSingleWithParameterized testMethod=test_wc_single_only_lines>, _ = 'only_lines', kwargs = {'lines': True, 'path': 'file.txt'}, expected = {'lines': 6}, stdout = False

        @parameterized.expand(get_params(), name_func=custom_name_func)
        def test_wc_single(self, _, kwargs, expected, stdout):
            with patch("sys.stdout", new=StringIO()) as output:
                res = wc_single(**kwargs)

                if stdout:
                    assert expected in output.getvalue().strip()
                else:
    >               assert expected == res
    E               AssertionError: assert {'lines': 6} == {'lines': 5}
    E                 Differing items:
    E                 {'lines': 6} != {'lines': 5}
    E                 Full diff:
    E                 - {'lines': 5}
    E                 ?           ^
    E                 + {'lines': 6}
    E                 ?           ^

    wc.py:96: AssertionError

### Pytest parametrize

<a href="https://docs.pytest.org/en/latest/parametrize.html" target="_blank">pytest</a> supports parametrizing test functions and not subclass methods of `` unittest.TestCase ``. <a href="https://github.com/wolever/parameterized" target="_blank">parameterize</a> library support all Python testing framework. You can mix and play with test functions, test classes, test methods. And pytest _only supports UK spelling_ `` paramterize `` whereas _parameterize library supports American spelling `` parameterize ``_. Pytest refused to support both the spellings.

<a href="https://www.youtube.com/watch?v=2R1HELARjUk" target="_blank">In recent PyCon 2020, there was a talk about pytest parametrize</a>. It’s crisp and provides a sufficient quick introduction to parametrization.

### Why parameterize tests?

1.   It follows DRY(Do not Repeat Yourself) principle.
2.   Changing and managing the tests are easier.
3.   In a lesser number of lines, you can test the code. At work, for a small sub-module, the unit tests took 660 LoC. After parameterization, tests cover only 440 LoC.

Happy parameterization!

### Important links from the post:

1.   Parameterized - <a href="https://github.com/wolever/parameterized" target="_blank">https://github.com/wolever/parameterized</a>
2.   Pytest Parametrize - <a href="https://docs.pytest.org/en/latest/parametrize.html" target="_blank">https://docs.pytest.org/en/latest/parametrize.html</a>
3.   PyCon 2020 Talk on Pytest Parametrize - <a href="https://www.youtube.com/watch?v=2R1HELARjUk" target="_blank">https://www.youtube.com/watch?v=2R1HELARjUk</a>
4.   Complete code - <a href="https://gitlab.com/snippets/1977169" target="_blank">https://gitlab.com/snippets/1977169</a>
