#!/bin/bash
#run ./build.sh on another terminal window
#it will clone your existing repo and run the maven tests off this clone
#the branch tests are run from is the current branch
#
# ./build.sh

#the cloned repo will live in ../DIRECTORY_ROOT/REPO_DIRECTORY
DIRECTORY_ROOT="../privatebuild/"

#get the lastest part of the directory name
IFS="/"
SPLIT_DIR=(`pwd`)
SIZE=${#SPLIT_DIR[@]}
let LAST_INDEX=$SIZE-1
DIRECTORY_SUFFIX=${SPLIT_DIR[$LAST_INDEX]}
IFS=""

DIRECTORY="${DIRECTORY_ROOT}${DIRECTORY_SUFFIX}"

BRANCH=`git branch | grep "*" | awk '{print $NF}'`

if [ ! -d "${DIRECTORY}" ]; then
  git clone . $DIRECTORY
fi

cd $DIRECTORY
#reset potential changes
git clean -df
#fetch repo
git fetch origin
#get master branch
git checkout master
if [ $? -eq 0 ]; then
  echo ""
else 
  #if it fails, get a new master from origin
  git checkout -b master origin/master
fi

git pull
#if requested branc is not master, get it
if [ "master" != $BRANCH ]; then
  git branch -D $BRANCH
  git checkout -b $BRANCH origin/$BRANCH
fi

echo ""
echo "***** Working on branch $BRANCH *****"
echo ""

if [ -e "pom.xml" ]; then
  mvn clean install

  if [ $? -eq 0 ]; then
    echo "Build results"
#    git push $REMOTE_REPO master
  else
    echo "Unable to build"
    exit $?
  fi
fi