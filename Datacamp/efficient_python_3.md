## Efficient Python Notes 3
### Pandas DataFrame iteration
### .iterrows() Method
The function iterrows() iterates through each row of the pandas dataframe. It returns the index of the row and the contents of the row as a tuple of the index and the pandas.Series of contents. Thus, we can have code such as:
```
for i, row in baseball_df.iterrows():
  wins = row['W']
  games_played = row['G']
  
  win_perc = calc_win_perc(wins, games_played)
  win_perc_list.append(win_perc)
  
baseball_df['WP']=win_perc_list
```
### .itertuples() Method
The next function is itertples(). It iterates through each row and returns a tuple with each attribute as the tuple name. For example:
```
for row_namedtuple in team_wins_df.itertuples():
  print(row_namedtuple)
```
This gives the following output:
```
Pandas(Index=0, Team='ARI', Year=2012, W=81)
Pandas(Index=1, Team='ATL', Year=2012, W=94)
...
```
We can then use the dot method to extract contents of the tuple:
```
print(row_namedtuple.Index)
print(row_namedtuple.Team)
```
.itertuples() is more efficient than .iterrows(). Pandas Series is overhead. However, it does not support [] indexing.
```
print(row_namedtuple['Team']
```
This gives an error. However, in iterrows(), the following line is all right:
```
print(row_tuple[1]['Team']
```

### .apply() Method
A more efficient way to calculate row['RS']-row['RA'] is to use the apply method that can also be used with lambda functon.
```
run_diffs_apply = baseball_df.apply(
  lambda row: calc_run_diff(row['RS'], row['RA']),
  axis=1
)
baseball_df['RD'] = run_diffs_apply
```
We can only apply to specific rows of the DataFrame:
```
total_runs = rays_df[['RS','RA]].apply(sum,axis=1)
```
### Using .values to convert to Numpy array
The .values attribute behind a column can convert that column into a Numpy array. We can broadcast using that attibute.
```
run_diffs_np = baseball_df['RS'].values - baseball_df['RA'].values
baseball['RD'] = run_diffs_np
```
