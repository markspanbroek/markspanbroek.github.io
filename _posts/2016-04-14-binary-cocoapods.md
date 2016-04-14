---
title: Binary CocoaPods
layout: post
---

When working in an enterprise environment, you sometimes cannot get away with distributing the source code of a framework. But luckily that doesn’t mean that you need to forgo proper dependency management. 

In our company we use CocoaPods for managing dependencies of our iOS apps and frameworks. Although CocoaPods is mostly geared towards distributing source code, it does have provisions for dealing with binary frameworks. I will outline a strategy for managing binary frameworks with CocoaPods below.

### Distributing your binary framework

When you build a binary framework, you can make it available for download by zipping the framework and putting it on a website, either on the internet or on the enterprise network. Just keep in mind that the website has to be reachable to every developer that types `pod update`.

If your binary framework is small you can also choose to add the binary to a git/svn/hg repository, although I would advise against adding the binaries to the same repository that contains the sources.

CocoaPods can retrieve binary frameworks using the same methods that it can retrieve sources. For a complete overview you can reference the “source” section of the
[Podspec Syntax Reference](https://guides.cocoapods.org/syntax/podspec.html)

### Referencing the binary framework from your podspec

Now that you have made the binary framework available for download you can create a podspec. This is similar to creating a podspec for a framework consisting of source files. You simply leave out the any source file related declarations such as `source_files`, and insert an appropriate `vendored_frameworks` declaration.

For example:

```ruby
Pod::Spec.new do |s|
  s.name = 'BananaLib'
  s.version = '1.0'
  s.author = 'Banana Corp'
  s.license = 'Proprietary'
  s.homepage = 'http://banana-corp.local/banana-lib.html'
  s.summary = 'Chunky bananas!'
  
  s.source = { 
    :http => 'http://banana-corp.local/banana-framework.zip'
  }
  s.vendored_frameworks = 'BananaFramework.framework'
end
```

If your framework is meant to be distributed within your enterprise only, then you probably need to [setup a private CocoaPod Spec repo](http://guides.cocoapods.org/making/private-cocoapods.html) within your enterprise network.

If on the other hand your framework is for general consumption, you can [submit your podspec to the main CocoaPods repo](https://guides.cocoapods.org/making/specs-and-specs-repo.html). 

### Combining binary and sources in a single podspec

For us the need arose to release a framework as source code to our internal developers, but restrict contracted developers to only work with the binary framework.

We managed to do this by introducing two subspecs, one for the source release, and one for the binary release.

Example:

```ruby
Pod::Spec.new do |s|
  s.name = 'BananaLib'
  s.version = '1.0'
  s.author = 'Banana Corp'
  s.license = 'Proprietary'
  s.homepage = 'http://banana-corp.local/banana-lib.html'
  s.summary = 'Chunky bananas!'

  s.default_subspec = 'Source'
  
  s.subspec 'Source' do |source|
    source.source = {
      :git => 'http://banana-corp.local/banana-lib.git',
      :tag => 'v1.0'
    }
    source.source_files = '**/*.swift'
  end
  
  s.subspec 'Binary' do |binary|
    binary.source = { 
      :http => 'http://banana-corp.local/banana-framework.zip'
    }
    binary.vendored_frameworks = 'BananaFramework.framework'
  end
  
end
```

A developer can now choose to integrate your framework as sources:

    pod 'BananaLib/Source'

Or as a binary:

    pod 'BananaLib/Binary'

Note that in our example we chose to set the “Source” subspec as the default subspec. When a developer references your framework in the Podfile without specifying the subspec (e.g. `pod 'BananaLib'`) they would get the sources. You could of course also choose to make the binary release the default.
