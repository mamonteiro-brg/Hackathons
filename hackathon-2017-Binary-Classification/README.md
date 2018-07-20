# What to do

Read the [challenge](https://drive.google.com/a/james.finance/file/d/0Bwi23MPVI4CtdGJVMGlfcktwcHc/view?usp=sharing).

# How to make submissions

1. Fill in the key variable with the key given to you by sam
2. Register this function in the same notebook that you
   have the dataframe with your predictions
3. Call `submit(probas_df)`

```python
import json
from urllib import request
from urllib.error import HTTPError

key = 'YOUR_KEY_HERE!!'


def submit(probas_df):
    """
    Call this function with a dataframe that contains the probabilities
    computed on the hackathon X_test and the urlids that each one corresponds
    to.
    :param probas_df: pandas dataframe with two columns 'urlid' and 'probas'
    :return: None
    """
    if not set(probas_df.columns) == {'urlid', 'probas'}:
        raise RuntimeError('df with columns urlid and probas required')
    probas_str = probas_df.reset_index(drop=True).to_csv(index=False).encode()
    try:
        req = request.Request(
            'http://hackathon.lisbondatascience.org/submit/{}/'.format(key),
            data=probas_str)
        response = request.urlopen(req)
        score = json.loads(response.read().decode())
        print('great success', score)
    except HTTPError as e:
        print('you messed up homie "{}"'.format(e.file.read().decode()))

```
