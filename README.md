# COMP3011 Assignment 2 – Multi threaded Video Player

## GitHub Repository

https://githubcom/BenUni67/VideoPlayerMultiThread

## Project Overview

This project is a Java video player that has been modified from the supplied single threaded starter code

The assignment involved two main tasks:

1 **Part 1:** Allow users to select video effects through command line arguments
2 **Part 2:** Implement multithreading to improve video processing performance while maintaining smooth playback

## Application Structure

The application follows a Model View Controller (MVC) design

The main classes are:

**VideoPlayerApp:** Starts the Java fx application and sets up the interface
**VideoPlayerController:** Connects the user interface to the video player model
**CommandLineController:** Reads command line arguments and determines which video effects to apply
**VideoPlayerModel:** Handles video decoding, playback, audio timing and multithreaded frame processing
**FrameProcessor:** Base class for the different video effects
**FrameProcessorObject:** Creates separate copies of video effects for different worker threads
**AudioPlayer:** Handles audio playback

## Design Changes

### Part 1 – Command Line Effects

The original application required effects to be user enabled in the source code

This was changed so users can choose effects using command line arguments

The application now supports all 11 effects, including repeated effects, stacked arguments and different effect orders

A list of `FrameProcessor` objects stores the selected effects and passes them through to `VideoPlayerModel`

### Part 2 – Multithreading

The original player processed video frames in order, which caused performance issues when multiple effects were applied

The following changes were made:

Added an `ExecutorService` thread pool to process frames on multiple threads
Used `CompletableFuture` to run frame processing in the background
Separated frame decoding from applying effects
Created independent frame copies to avoid threads modifying the same frame
Gave worker threads their own sets of frame processors to avoid shared state issues
Limited the number of frames being processed at once to avoid excessive resource usage
Stored completed frames and displayed them in the correct order
Added playback checks to prevent old frames from appearing after seeking or stopping
Updated frame timing to support pausing and resuming correctly

These changes allow multiple video frames to be processed at the same time, making better use of available CPU cores

Tests were added to check important functionality

**CommandLineControllerTest**

Tests multiple command line arguments
Tests mixed short and long arguments
Tests invalid command line arguments

**FrameProcessorObjectTest**

Tests that separate processor objects are created
Tests that effect order and duplicates are preserved

The application was also tested using TITAN to compare its performance against the original single threaded player

TITAN testing demonstrated improved frame processing performance and the ability to utilise multiple CPU cores
