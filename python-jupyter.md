## jupyter
- tips https://towardsdatascience.com/7-essential-tips-for-writing-with-jupyter-notebook-60972a1a8901
- themes https://github.com/dunovank/jupyter-themes

```python
!jt -r # default
!jt -t monokai -f roboto -fs 10 -T -N -tf fira -tfs 10 -nf fira -nfs 11 # monokai/roboto/fira
```

### run unittest

https://medium.com/@vladbezden/using-python-unittest-in-ipython-or-jupyter-732448724e31

```python
if __name__ == '__main__':
    unittest.main(argv=['first-arg-is-ignored'], exit=False)
```

### convert to classic html

```bash
jupyter nbconvert --to=html --template=classic NAME.ipynb
```
