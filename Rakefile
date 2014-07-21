require 'rake/clean'

# include FileUtils

ORIGINAL_IMAGES = FileList['images/original/*.{png,jpg}']
DESIRED_SIZES = [720,360]
resized_files = []
DESIRED_SIZES.each do |px_size|
  directory "images/resized/#{px_size}px"
  sized_pathmap = ORIGINAL_IMAGES.pathmap("images/resized/#{px_size}px/%f")
  sized_pathmap.zip(ORIGINAL_IMAGES).each do |target, source|
    file target => ["images/resized/#{px_size}px", source] do
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

# destroy the fuck out of the images with pngquant!
# make this efficient with file tasks later, its fast enough for now to just rerun
task 'images:pngquant' => 'images:resize' do
  sh "find images/resized -name '*.png' -print0 | xargs -0 -P8 -n1 pngquant --force --ext .png --speed 1"
end