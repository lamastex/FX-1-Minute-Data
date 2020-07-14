# Steps to download data and copy to s3

## 1. clone git-repo, and then pull and run docker container

```
git clone git@github-raaz:lamastex/FX-1-Minute-Data.git
docker pull lamastex/python-findata
docker run --rm  -it --mount type=bind,source=${PWD},destination=/root/GIT lamastex/python-findata /bin/bash
```

Then inside docker container:

- `cd` into `/root/GIT/FX-1-Minute-Data`
- and run `python download_all_fx_data.py` so latest data is downloaded to `output` directory

Now you need to `unzip` and re-compress in an open format such as `.gz` instead of `.zip` and then copy the files to s3 or other storage.
Get out of docker as you need `aws` CLI outside of the public docker image used here.

## 2. zip -> gz -> s3

```
cd output
find . -name "*.zip" | while read filename; do sudo unzip -o -d "`dirname "$filename"`" "$filename"; done;
sudo find . -type f ! -name '*.zip' -exec gzip "{}" \; # also try simpler: gzip -r ./
find * -name "*.gz" | while read filename; do aws s3 cp $filename s3://xxxxxxxxxxxxxxxxx/findata/com/histdata/free/FX-1-Minute-Data/$filename; done;
```
