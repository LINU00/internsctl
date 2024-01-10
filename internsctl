#!/bin/bash

# Name: Aditya Singh
# internsctl - A simple command-line tool for basic system tasks

function usage() {
    echo "Usage: internsctl [OPTION]... [FILE]..."
    echo "internsctl is a high-level shell program for performing various tasks."
    echo
    echo "Options:"
    echo "  cpu getinfo                   Display information about CPU architecture."
    echo "  file getinfo <file-name>      Display information about the file."
    echo "  file getinfo [OPTION] <file-name>  Display specific information about the file."
    echo "    --last-modified, -m         Display the last modified time of the specified file only."
    echo "  memory getinfo                Display the amount of free and used memory in the system."
    echo "    --owner, -o                 Display the owner of the specified file only."
    echo "    --permissions, -p           Display the permissions of the specified file only."
    echo "    --size, -s                  Display the size of the specified file only."
    echo "  user create <user-name>       Create a new user with the entered username."
    echo "  user list                     List all the users in the server."
    echo "    --sudo-only                 List all the users with sudo permissions."
    echo "  --version                     Output version."
}

function internsctl() {
  version="v0.1.0"

  # Print version information
  if [ "$1" = "--version" ]; then
    echo "version: $version"
  
  # Display help information
  elif [ "$1" = "--help" ]; then
    usage

  # Get CPU information
  elif [ "$1" = "cpu" ] && [ "$2" = "getinfo" ]; then
    lscpu

  # Get memory information
  elif [ "$1" = "memory" ] && [ "$2" = "getinfo" ]; then
    free

  # Create a new user
  elif [ "$1" = "user" ] && [ "$2" = "create" ]; then
    sudo adduser "$3"

  # List users with optional sudo-only flag
  elif [ "$1" = "user" ] && [ "$2" = "list" ]; then
    if [ "$3" = "--sudo-only" ]; then
      grep '^sudo:.*$' /etc/group | cut -d: -f4
    else
      cat /etc/passwd | cut -d: -f1
    fi

  # Get file information
  elif [ "$1" = "file" ] && [ "$2" = "getinfo" ]; then
    if (( $# < 3 )); then
      echo "Invalid command"
      return
    fi

    file_path="${@: -1}"

    # Check if the file exists
    if [ ! -f "$file_path" ]; then
      echo "This file doesn't exist"
      return
    fi

    case $3 in
      "--size" | "-s")
        ls -l "$file_path" | awk '{print $5}'
        ;;
      "--permissions" | "-p")
        ls -l "$file_path" | awk '{print $1}'
        ;;
      "--owner" | "-o")
        ls -l "$file_path" | awk '{print $3}'
        ;;
      "--last-modified" | "-m")
        stat -c %y "$file_path"
        ;;
      *)
        owner=$(ls -l "$file_path" | awk '{print $3}')
        size=$(ls -l "$file_path" | awk '{print $5}')
        permission=$(ls -l "$file_path" | awk '{print $1}')
        lastModified=$(stat -c %y "$file_path")

        echo "File: $file_path"
        echo "Access: $permission"
        echo "Size(B): $size"
        echo "Owner: $owner"
        echo "Modify: $lastModified"
        ;;
    esac

  # Invalid arguments
  else
    echo "Error: Invalid arguments provided. Please check usage by using --help and provide valid arguments."
  fi
}

# Call the internsctl function with command-line arguments
internsctl "$@"

