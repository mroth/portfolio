require 'rake/clean'

# include FileUtils

ORIGINAL_IMAGES = FileList['images/original/*.{png,jpg}']
DESIRED_SIZES = [720,360]
resized_files = []
DESIRED_SIZES.each do |px_size|
  directory "images/#{px_size}px"
  sized_pathmap = ORIGINAL_IMAGES.pathmap("images/#{px_size}px/%f")
  sized_pathmap.zip(ORIGINAL_IMAGES).each do |target, source|
    file target => ["images/#{px_size}px", source] do
      sh "convert #{source} -resize #{px_size}x#{px_size}\\> #{target}"
    end
  end
  resized_files << sized_pathmap
end

# build a file list of the resulting resized images, for mapping and clobber
RESIZED_IMAGES = resized_files.inject(:+)
CLOBBER.include(RESIZED_IMAGES)

desc "created resized images"
task 'images:resize' => RESIZED_IMAGES
