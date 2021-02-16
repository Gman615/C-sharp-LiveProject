# C# Live Project

As a part of my programming education through the Tech Academy Boot Camp, we did a two week live project that involved an MVC app for a fictional theatre. With this project, I worked with a group of other students in adding functionality to this page. The camp used the agile and scrum methodology, and we conducted daily standup meetings to get experience with what it is like working with a software development team. We were also assigned stories to get practice with the daily routine of a software developer. Below are some snippets from my work on this project.

# Cast Member Model

Below is the model I created for the Cast Member section of the site that I created for this live project. This section allows site users to input data about cast members who are involved in the theatre productions. There is also a property which allows users to upload a photo and it is converted to bytes and stored in the database.

    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Linq;
    using System.Web;

    namespace TheatreCMS2.Models
    {
        public class CastMember
        {
            public int CastMemberId { get; set; }
            public string Name { get; set; }
            public int? YearJoined { get; set; }
            public Position MainRole { get; set; }
            public string Bio { get; set; }
            public byte[] Photos { get; set; }
            public bool CurrentMember { get; set; }
            public string Character { get; set; }
            public int? CastYearLeft { get; set; }
            public int? DebutYear { get; set; }

        }

        public enum Position 
        { 
            Actor,
            Director,
            Technician,
            StageManager,
            Other
        }
    }

# Controller

The below section is the section from my controller which converts the provided image into bytes, then stores it in the database. After this, the image is converted from bytes back into an image and displayed on the page.

        // converts image files to byte array
        public byte[] ImageToByteArray(HttpPostedFileBase postedFile)
        {
            CastMember castMember = new CastMember();
            byte[] bytes;
            using (BinaryReader br = new BinaryReader(postedFile.InputStream))
            {
                bytes = br.ReadBytes(postedFile.ContentLength);
            }
            return bytes;
        }
        
        //takes images from the db and converts them into a file content result
        public FileContentResult RetrieveImage(int id)
        {           
            byte[] byteData = db.CastMembers.Find(id).Photos;
            FileContentResult fileName = File(byteData, "image/jpeg");
            return fileName;
        }
