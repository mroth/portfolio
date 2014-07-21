require 'image_optim'
require 'rake/clean'

# include FileUtils

ORIGINAL_IMAGES = FileList['images/projects/original/*.{png,jpg}']
DESIRED_SIZES = [720,360]
resized_files = []
DESIRED_SIZES.each do |px_size|
  directory "images/projects/resized/#{px_size}px"
  sized_pathmap = ORIGINAL_IMAGES.pathmap("images/projects/resized/#{px_size}px/%f")
  sized_pathmap.zip(ORIGINAL_IMAGES).each do |target, source|
    file target => ["images/projects/resized/#{px_size}px", source] do
      sh "convert #{source} -resize #{px_size}x#{px_size}\\> #{target}"
    end
  end
  resized_files << sized_pathmap
end

# build a file list of the resulting resized images, for mapping and clobber
RESIZED_IMAGES = resized_files.inject(:+)
CLOBBER.include(RESIZED_IMAGES)

desc "created resized images"
multitask 'images:resize' => RESIZED_IMAGES

# build a file list of just the resized pngs, since we need it for the annoying
# manual pngquant tasks
RESIZED_PNGS = FileList.new(RESIZED_IMAGES)
RESIZED_PNGS.exclude("**/*.jpg")
RESIZED_PNGS.exclude("**/*.gif")

desc "safe pngquant appropriate resized images"
multitask 'images:quantize' => 'images:resize' do
  RESIZED_PNGS.each do |f|
    sh "pngquant --skip-if-larger --force --ext .png --speed 1 #{f}"
  end
end

desc "lossless optimization for all resized images"
task 'images:optimize' => 'images:resize' do
  image_optim = ImageOptim.new(:pngout => false)
  image_optim.optimize_images!(Dir['images/projects/resized/**/*.*'])
end

desc "build the site with jekyll"
task 'jekyll:build' do
  sh "jekyll build"
end

desc "check links locally"
task 'checklinks:local' => 'jekyll:build' do
  sh "check-links '_site'"
end

desc "check links in prod"
task 'checklinks:prod' do
  sh "open 'http://validator.w3.org/checklink?uri=http%3A%2F%2Fportfolio.mroth.info&hide_type=all&depth=&check=Check'"
end
